# TUTO 리눅스 튜토리얼

### 역할분담
성민수 : 전체적인 ui작업, 튜토리얼 작업공간 관리(사용자가 튜토리얼 명령어 사용하는 내부 폴더)
윤민성 : 기본 셸 명령어 튜토리얼 리워크(기존 학습 기능 개선)
정래현 : 텍스트 편집기 튜토리얼 추가(nano 또는 vim 중 택1)


### 요약:  
tuto는 리눅스 설치를 마쳤으나 터미널 사용에 익숙하지 않은 사용자를 위한 튜토리얼 도구다. 웹사이트나 별도의 앱을 열지 않고, 특별한 서버 없이 가볍게 시작하고 터미널 조작에 익숙해질 수 있다. 설치된 터미널에서 tuto list로 준비된 튜토리얼 목록을 확인할 수 있으며 tuto 1과 같이 번호를 입력하여 튜토리얼을 시작할 수 있다.
이번 프로젝트에서는 ls, cd, mkdir과 같은 기본 명령어와 nano, vim을 이용한 텍스트 편집기 튜토리얼을 구현한다. git, tmux와 같은 도구의 학습 과정은 이후 확장 대상으로 둔다. 사용자는 가이드를 따라 직접 실습하며, 튜토리얼 시작 시 필요한 도구의 설치 안내와 전용 연습 디렉터리, 단계별 힌트를 제공받는다.
이를 통해 입문자가 별도의 학습 자료를 찾아보지 않고도 터미널 안에서 명령어를 직접 익히고 실습 결과를 확인할 수 있다.

### 서론:  
기존 문서형 가이드를 이용할 때 입문자는 자신에게 필요한 기능과 명령어를 하나씩 찾아보고, 해당 설명을 실제 작업에 어떻게 적용할지 판단해야 한다. 아직 명령어 이름이나 도구의 사용 방식에 익숙하지 않은 사용자에게는 무엇을 찾아야 하는지 결정하는 것부터 부담이 될 수 있다. 익숙해지기 전까지 가이드를 반복해서 찾아 열고 터미널로 돌아오는 과정은 학습과 실습의 흐름을 끊고 접근을 번거롭게 만든다.

본 프로젝트의 대상은 리눅스 설치를 마쳤지만 터미널 사용 경험이 부족한 입문자다. 이들에게 필요한 것은 개별 기능의 설명뿐 아니라, 무엇부터 익히고 어떻게 연습할지 안내하는 학습 흐름이다. 필요한 내용을 직접 검색하고 별도의 문서나 창을 오가야 하는 부담을 줄이며, 필요한 기능을 현재 작업 중인 터미널에서 쉽게 숙달할 수 있도록 하는 것을 목표로 하였다.

이를 위해 별도의 웹페이지나 여분의 창을 열지 않고, 현재 터미널 안에서 학습과 실습을 모두 수행할 수 있는 튜토리얼 도구 tuto를 제안한다. tuto는 입문자에게 적절한 학습 흐름과 실습기회를 제공하며, 입문자가 아닌 중~상급자 또한 tuto를 통해 사용법이 잘 기억나지 않는 도구를 다시 쉽게 사용할 수 있다.

### 본론:
시스템은 로컬 명령줄 실행기, 튜토리얼 콘텐츠, 실습 실행기, 결과 평가기와 진도 저장소로 구성한다. tuto list는 번호별 주제와 완료 여부를 표시하고, tuto (숫자)는 각각 해당 숫자 번호에 맞는 주제로 튜토리얼을 시작한다.
구현 후보는 Python 기반 명령줄 프로그램이다. argparse로 list와 번호 인자를 해석하고, JSON으로 설명·목표·힌트·초기 파일·평가 규칙을 정의한다. subprocess로 설치된 Bash·nano·Vim·tmux 등을 실행하며 현재 터미널의 입력·출력을 그대로 연결한다. 학습 로그는 사용자별 로컬 JSON 파일로 저장한다.

최소 기능은 tuto list, tuto (숫자), 설명 출력, 연습 파일 생성과 결과 검사다. 기본 명령어 과정은 준비된 디렉터리에서 대화형 셸을 열고 사용자가 exit로 돌아오면 평가한다. 편집기 과정은 편집기를 종료한 뒤 파일 내용을 검사한다. tmux 과정은 앱으로 돌아온 뒤 학습 세션·창 상태를 검사한다. 이후 힌트·재시도·진도 저장을 추가하며 잘못된 번호와 실행 중단도 처리한다. 시작 시 필요한 도구의 설치 여부를 확인하고 누락된 도구의 설치 방법을 안내하되 자동으로 설치하지 않는다.

기존 유사 프로젝트인 GameShell을 오픈소스 활용 후보로 검토한다. GameShell과 같은 오픈소스의 터미널 기반 미션 진행 및 실습 결과 확인 구조를 분석하고, 실제 활용 가능성과 라이선스를 확인 후 적용 범위를 결정한다. 이를 바탕으로 기존 기능을 재현하고 한국어 학습 안내와 리눅스 기초 명령어 및 텍스트 편집기 실습 등 tuto만의 기능을 추가하는 방향으로 개발한다.



아래는 Gameshell의 기존 README

GameShell: a "game" to teach the Unix shell
===========================================

![Illustration inspired by the game](Images/illustration-small.png)

Teaching first-year university students or high schoolers to use a Unix shell
is not always the easiest or most entertaining of tasks. GameShell was devised
as a tool to help students at the
[Université Savoie Mont Blanc](https://univ-smb.fr) to engage with a *real*
shell, in a way that encourages learning while also having fun. 

The original idea, due to Rodolphe Lepigre, was to run a standard bash session
with an appropriate configuration file that defined "missions" which would be
"checked" in order to progress through the game.

Here is the result...

![GameShell's first mission](Images/gameshell_first_mission_small.gif)


GameShell is available in English, French and Italian.


Feel free to send us your remarks, questions or suggestions by opening
[issues](https://github.com/phyver/GameShell/issues) or submitting
[pull requests](https://github.com/phyver/GameShell/pulls).
We are particularly interested in any new missions you might create!


Getting started
---------------

GameShell should work on any standard Linux system, and also on macOS and BSD
(but we have run fewer tests on the latter systems). On Debian or Ubuntu, the
only dependencies (besides `bash`) are the `gettext-base` and `awk` packages
(the latter is generally installed by default). Some missions have additional
dependencies: these missions will be skipped if the dependencies are not met.
On Debian or Ubuntu, run the following command to install all game and mission
dependencies.
```sh
$ sudo apt install gettext man-db procps psmisc nano tree ncal x11-apps wget
```
Check the [user manual](doc/user_manual.md) to see how to install the game
dependencies on other systems (macOS, BSD, ...).

Assuming all the dependencies are installed, you can try the latest version of
the game by running the following two commands in a terminal.
```sh
$ wget https://github.com/phyver/GameShell/releases/download/latest/gameshell.sh
$ bash gameshell.sh
```
The first command will download the latest version of the game in the form of
a self-extracting archive, and the second command will initialise and run the
game from the downloaded archive. Instructions on how to play are provided in
the game directly.

Note that when you quit the game (with `control-d` or the command `gsh exit`)
your progression will be saved in a new archive (called `gameshell-save.sh`).
Run this archive to resume the game where you left it.


If you prefer not running foreign shell scripts on your computer, you can
generate a Docker image with the following:
```sh
$ mkdir GameShell; cd GameShell
$ wget --quiet https://github.com/phyver/GameShell/releases/download/latest/Dockerfile
$ docker build -t gsh .
$ docker run -it gsh
```
The game will NOT be saved when you exit, and additional flags are required if
you want to run X programs from inside GameShell. Refer to [this
section](./doc/deps.md#running-GameShell-from-a-docker-container) of the user
manual.

Github Codespaces (or VSCode)
-----------------------------

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/phyver/GameShell)

This repository is configured to work with the Visual Studio Code
[Dev Container](https://containers.dev/) extension, which enables GameShell to
be run from a [Github Codespace](https://github.com/features/codespaces).

Once the Codespace is launched (via the above badge), you can run GameShell in
the terminal with the following command.
```sh
bash start.sh
```
An alternative language can be selected using the `-L` option. For example,
the following command will start the game in Italian.
```sh
bash start.sh -L it
```

To get the same experience on your local machine without the limitations/cost
of Codespaces see the [Dev Container docs](https://containers.dev/supporting#tools).

Documentation
-------------

To find out more about GameShell, refer to the following documents:
- The [user manual](doc/user_manual.md) provides information on how to run the
  game on all supported platforms (Linux, macOS, BSD), explains how to run the
  game from the sources, tells you how to generate custom game archives (which
  is useful if you want to use GameShell for teaching a class), and more.
- The [developer manual](doc/dev_manual.md) provides information on how to
  create new missions, how to translate missions, and how to participate
  in the development of the game.


Who is developing GameShell?
----------------------------

### Developers

The game is currently being developed by:
* [Pierre Hyvernat](http://www.lama.univ-smb.fr/~hyvernat) (main developer,
  [pierre.hyvernat@univ-smb.fr](mailto:pierre.hyvernat@univ-smb.fr)),
* [Rodolphe Lepigre](https://lepigre.fr).

### Mission contributors

* Pierre Hyvernat
* Rodolphe Lepigre
* Christophe Raffalli
* Xavier Provencal
* Clovis Eberhart
* Sébastien Tavenas
* Tiemen Duvillard

### Translation

#### Italian Version

* Daniele Scasciafratte (@mte90)
* Paolo Mauri (@maupao)
* Marco Ciampa (@ciampix)
* Antonio Vivace (@avivace)
* Lorenzo Millucci (@lmillucci)
* Sirio Negri (@ziriuz84)
* Domenico Mammola (@domenicomammola)
* Leonardo Canello (@anulo2)
* @michirod
* @serhack
* WhiteShield (@wshield05)
* @gioisco

### Special thanks

* All the students who found *many* bugs in the early versions.
* Joan Stark (a.k.a, jgs), who designed hundreds of ASCII-art pieces in the
  late 90s. Most of the ASCII-art encountered in GameShell are due to her.


Licence
-------

GameShell is released under the [GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html).

Please link to this repository if you use GameShell.

GameShell is open source and free to use. One way you can acknowledge the work
it required is by sending an actual postcard to

```
  Pierre Hyvernat
  Laboratoire de Mathématiques, CNRS UMR 5127
  Université de Savoie
  73376 Le Bourget du Lac
  FRANCE
```

