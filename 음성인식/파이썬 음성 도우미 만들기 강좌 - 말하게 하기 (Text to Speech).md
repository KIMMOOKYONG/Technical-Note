# 참고
- https://wikidocs.net/15213
- (음성인식 및 음성 생성)https://davey.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9D%8C%EC%84%B1-%ED%85%8D%EC%8A%A4%ED%8A%B8-%EB%B3%80%ED%99%98-%EB%82%B4-%EB%A7%90-%EB%8C%80%EB%8B%B5-AI-%EB%A1%9C%EB%B4%87-%EB%A7%8C%EB%93%A4%EA%B8%B0-gTTs-SpeechRecognition-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC
- https://www.youtube.com/watch?v=QZmjzVRjFD8&list=PL4HcB_xg04MraA7q5Io21NBLcy-s-sfsz&index=16
- https://www.shop2school.com/course/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9D%8C%EC%84%B1-%EB%8F%84%EC%9A%B0%EB%AF%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-%EA%B0%95%EC%A2%8C/
- https://www.youtube.com/watch?v=7BUK0pOUpKA
- https://fast-it.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9C%BC%EB%A1%9C-%EC%9D%8C%EC%84%B1%EC%9D%B8%EC%8B%9D-%EB%B4%87-%EB%A7%8C%EB%93%A4%EA%B8%B01
- 

# 패키지 설치
```
!pip install SpeechRecognition

```
![image](https://user-images.githubusercontent.com/102650331/172747415-a4148621-abb7-47e3-8d2d-4cca1ce9fc64.png)

```
!pip install gTTS

```

```
!pip install playsound

```

```
!apt install libasound2-dev portaudio19-dev libportaudio2 libportaudiocpp0 ffmpeg
!pip install pyAudio

```

# 윈도우 설치
```
# pyaudio 내력받기
- www.lfd.uci.edu/~gohlke/pythonlibs/#pyaudio
- PyAudio‑0.2.11‑cp37‑cp37m‑win32.whl
- pip install PyAudio‑0.2.11‑cp37‑cp37m‑win32.whl

```




# 음성 생성 코드
```python
import speech_recognition as sr
from gtts import gTTS
import os
import time
import playsound

def speak(text):
    tts = gTTS(text=text, lang="en")
    filename = "voice.mp3"
    tts.save(filename)
    playsound.playsound(filename)

speak("hello shop2word")

```

```
# 윈도우 환경 실행시 아래 오류 발생
Error 263 for command:
close voice.mp3
지정한 장치가 열려 있지 않거나 MCI에서 인식되지 않습니다.
Failed to close the file: voice.mp3
https://pythonmana.com/2022/02/202202141456530161.html
https://stackoverflow.com/questions/68531326/what-is-the-error-in-the-code-for-this-playsound-module-even-though-the-syntax-i

pip install playsound==1.2.2
이버전을 설치하면 해결됨

```


```python
import speech_recognition as sr
from gtts import gTTS
import os
import time
import playsound

def speak(text):
    tts = gTTS(text=text, lang="ko")
    filename = "voice.mp3"
    tts.save(filename)
    # 구글 코랩에서는 실행 안됨
    # playsound.playsound(filename)

speak("안녕하세요. 좋은 아침입니다.")

```

# 음성 인식 코드
```python
import speech_recognition as sr
from gtts import gTTS
import os
import time
import playsound

def speak(text):
    tts = gTTS(text=text, lang="ko")
    filename = "voice.mp3"
    tts.save(filename)
    # 구글 코랩에서는 실행 안됨
    # playsound.playsound(filename)

```

```python
def get_audio():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        audio = r.listen(source)
        said = ""
        
        try:
            said = r.recognize_google(audio)
            print(said)
        except Exception as e:
            print("Exception: " + str(2))
    return said
 
        
speak("hello")
text = get_audio()

if "hello" in text:
    speak("hello, how are you?")
elif "what is your name" in text:
    speak("My name is shop2world)
    
```

```python
import speech_recognition as sr
from gtts import gTTS
import os
import time
import playsound
import IPython
from IPython.display import Audio

def speak(text):
    tts = gTTS(text=text, lang="ko")
    filename = "voice.mp3"
    tts.save(filename)
    # 구글 코랩에서는 실행 안됨
    # playsound.playsound(filename)
    IPython.display.Audio("voice.mp3", autoplay=True)    

```

``` python
import speech_recognition as sr
from gtts import gTTS
import os
import time
import playsound

def speak(text):
    tts = gTTS(text=text, lang="ko")
    filename = "voice" + date_string + ".mp3"
    tts.save(filename)
    playsound.playsound(filename)

def get_audio():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        audio = r.listen(source)
        said = ""
        
        try:
            said = r.recognize_google(audio)
            print(said)
        except Exception as e:
            print("Exception: " + str(2))
    return said

speak("hello")
while True:
    text = get_audio()

    if "hello" in text:
        speak("hello, how are you?")
    elif "what is your name" in text:
        speak("My name is shop2world")

```

```
with open(str(savefile), "wb") as f:
PermissionError: [Errno 13] Permission denied: .mp3

응답 시 마다 새로운 파일 생성하는 방법으로 해결하라고 함
https://stackoverflow.com/questions/39818922/errno-13-permission-denied-file-mp3-python

```

```python
import speech_recognition as sr
from gtts import gTTS
import os
import time
import uuid
import playsound

def speak(text):
    tts = gTTS(text=text, lang="ko")
    filename = "voice" + str(uuid.uuid4()) + ".mp3"
    tts.save(filename)
    playsound.playsound(filename)

def get_audio():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        audio = r.listen(source)
        said = ""
        
        try:
            said = r.recognize_google(audio)
            print(said)
        except Exception as e:
            print("Exception: " + str(2))
    return said

speak("hello")
while True:
    text = get_audio()

    if "hello" in text:
        speak("hello, how are you?")
    elif "what is your name" in text:
        speak("My name is shop2world")

```
