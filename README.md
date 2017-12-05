# RapidSphinx
Android library for offline speech recognition.

## Features
- [x] Build dictonary on the fly
- [x] Build language model on the fly
- [x] Scoring system for every single word (range 0.0 - 10.0)
- [x] Detect unsupported words
- [x] Speaker adaptation (In progress)
- [x] SIMPLE TO USE!

## Preview
I have tried to speak in different word order:
<p align="center">
    <img width="350" src="https://github.com/icaksama/RapidSphinx/blob/master/RapidSphinxAppPreview.gif?raw=true">
</p>

## Gradle
Add to build.gradle :
```groovy
compile 'com.icaksama.rapidsphinx:master:1.+@aar'
```

# How to Use

## Add Request Permissions
Add permissions to your Manifest:
```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```
Add programs to your class/activity:
```java
private void requestPermissions() {
    if (ContextCompat.checkSelfPermission(this,
        Manifest.permission.RECORD_AUDIO) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this,
            new String[]{Manifest.permission.RECORD_AUDIO},
            1);
    }
}
```

## Add The Listener
First, impletent the RapidSphinxListener in your class/activity :
```java
public class MainActivity implements RapidSphinxListener {

    private RapidSphinx rapidSphinx;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        rapidSphinx = new RapidSphinx(this);
        rapidSphinx.addListener(this);
    }
}
```
RapidSphinxListener have some methods must be @Override in your class/activity :
```java
@Override
public void rapidSphinxDidStop(String reason, int code) {
    if (code == 500) { // 500 code for error
        System.out.println(reason);
    } else if (code == 522) { // 522 code for timed out
        System.out.println(reason);
    } else if (code == 200) { // 200 code for finish speech
        System.out.println(reason);
    }
}

@Override
public void rapidSphinxFinalResult(String result, List<Double> scores) {
    System.out.println(result);
    // Get score every single word
    for (double score: scores) {
        System.out.println(score);
    }
}

@Override
public void rapidSphinxPartialResult(String partialResult) {
    System.out.println(partialResult);
}

@Override
public void rapidSphinxUnsupportedWords(List<String> words) {
    String unsupportedWords = "";
    for (String word: words) {
        unsupportedWords += word + ", ";
    }
    System.out.println("Unsupported words : \n" + unsupportedWords);
}

@Override
public void rapidSphinxDidSpeechDetected() {
    System.out.println("Speech detected!");
}
```

## Prepare Speech Recognition
You need to prepare speech recognition before use that:
```java
rapidSphinx.prepareRapidSphinx(new RapidSphinxCompletionListener() {
    @Override
    public void rapidSphinxCompletedProcess() {
        System.out.println("Preparation was done!");
    }
});
```

## Update The Vocabulary
You can update the vocabulary on the fly:
```java
rapidSphinx.updateVocabulary("YOUR TEXT HERE!", new RapidSphinxCompletionListener() {
    @Override
    public void rapidSphinxCompletedProcess() {
        System.out.println("Vocabulary updated!");
    }
});
```

## Start The Speech Recognition
```java
// Set the Timeout in miliseconds
rapidSphinx.startRapidSphinx(10000);
```
- Please look at RapidSphinxDemo for detail.

## Additional Libraries
- [x] [RapidWave](https://github.com/icaksama/RapidWave)
- [x] [RapidGram](https://github.com/icaksama/RapidGram)

## MIT License
```
Copyright (c) 2017 Saiful Irham Wicaksana

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
