# Refactoring
From Thornthep's readability code: [https://github.com/Raikirieiei/PA4-Readability](https://github.com/Raikirieiei/PA4-Readability) <br/>
## In CountSyllable.java and CountWord.java (Same method)

### Method: IsVowel(String alphabet)
```
private boolean IsVowel(String alphabet) {
    return (alphabet.equalsIgnoreCase("a") || alphabet.equalsIgnoreCase("e") || alphabet.equalsIgnoreCase("i")
    || alphabet.equalsIgnoreCase("o") || alphabet.equalsIgnoreCase("u"));
}
```
Problems:<br/>
The return statement is too long and hard to read.<br/>
Wrong naming convention for the method.

Refactor:<br/>
Create a string constant for vowels and use it to check if it contains the alphabet.
Rename the method.

```
private boolean isVowel(String alphabet) {
    String vowels = "AEIOUaeiou";
    return (vowels.contains(alphabet));
}
```

Benefit:<br/>
The code is shorter and easier to read.

## In CountSyllable.java

### Method: OnlyE(String str)
```
private boolean OnlyE(String str) {}
```
Problems:<br/>
Wrong naming convention for the method. Some people will mistaken it for a class.

Refactor:<br/>
Rename the method.

```
private boolean onlyE(String str) {}
```

Benefit:<br/>
The code becomes easier to understand for other people since the naming convention is now correct.

## In FleschReadability.java

### Method: GradeCalculator(double index)
```
public String GradeCalculator(double index){
    if (index > 100)    return "4th grade student (elementary school) ";
    else if (90 <= index && index <= 100) return "5th grade student";
    else if (80 <= index && index <= 90) return "6th grade student";
    else if (70 <= index && index <= 80) return "7th grade student";
    else if (65 <= index && index <= 70) return "8th grade student";
    else if (60 <= index && index <= 65) return "9th grade student";
    else if (50 <= index && index <= 60) return "High school student";
    else if (30 <= index && index <= 50) return "College student";
    else if (0 <= index && index <= 30) return "College graduate";
    else return "Advance degree graduate";
}
```

Problems:<br/>
Wrong naming convention for the method.<br/>
The condition in each if else statement can be reduce.

Refactor:<br/>
Remove the condition after && because they are not needed.
Rename the method.

```
public String gradeCalculator(double index){
    if (index > 100)    return "4th grade student (elementary school) ";
    else if (index > 90) return "5th grade student";
    else if (index > 80) return "6th grade student";
    else if (index > 70) return "7th grade student";
    else if (index > 65) return "8th grade student";
    else if (index > 60) return "9th grade student";
    else if (index > 50) return "High school student";
    else if (index > 30) return "College student";
    else if (index >= 0) return "College graduate";
    else return "Advance degree graduate";
}
```

Benefit:<br/>
The code is shorter and easier to read.

## In ReadAndCount.java

### Method: Read(String source)
```
public void Read(String source) {
    if (source.contains("/")) {
        try {
            url = new URL(source);
        } catch (Exception e) {}
        try (BufferedReader br = new BufferedReader(new InputStreamReader(url.openStream()))) {
            while((line = br.readLine()) != null && !line.isEmpty()) {
                countWord += countWordStrat.Counting(line);
                countSyllable += countSyllableStrat.Counting(line);
                countSentence += countSentenceStrat.Counting(line);
            }
        } catch (IOException e) {ui.PopupError("File not found");}
    } else {
        File file = new File(source);
        if (!file.exists() || !file.isFile())
            ui.error("File does not exist or is not a regular file.");
        if (!file.canRead())
            ui.error("File is not readable");
        try (BufferedReader br = new BufferedReader(new FileReader(file))) {
            while((line = br.readLine()) != null) {
                countWord += countWordStrat.Counting(line);
                countSyllable += countSyllableStrat.Counting(line);
                countSentence += countSentenceStrat.Counting(line);
            }
        } catch (IOException e) {ui.PopupError("File not found");}
    }
}
```

Problems:<br/>
This method has too many function. It read the file and count word, syllable, and sentence.<br/>
Wrong naming convention for the method. 

Refactor:<br/>
Extract the counting into another method call count().
Rename the method.

```
public void count(String line) {
    countWord += countWordStrat.Counting(line);
    countSyllable += countSyllableStrat.Counting(line);
    countSentence += countSentenceStrat.Counting(line);    
}

public void read(String source) {
    if (source.contains("/")) {
        try {
            url = new URL(source);
        } catch (Exception e) {}
        try (BufferedReader br = new BufferedReader(new InputStreamReader(url.openStream()))) {
            while((line = br.readLine()) != null && !line.isEmpty()) {
                count(line);
            }
        } catch (IOException e) {ui.PopupError("File not found");}
    } else {
        File file = new File(source);
        if (!file.exists() || !file.isFile())
            ui.error("File does not exist or is not a regular file.");
        if (!file.canRead())
            ui.error("File is not readable");
        try (BufferedReader br = new BufferedReader(new FileReader(file))) {
            while((line = br.readLine()) != null) {
                 count(line);
            }
        } catch (IOException e) {ui.PopupError("File not found");}
    }
}
```

Benefit:<br/>
The code become more readable. It is easier to understand the functionality of each method.