# 시작하기전에
이 베젤 파일은 하드커널사의 ODROID GO SUPER 사용자를 위한 파일입니다

베젤 설정 파일과 베젤 파일들은 EmuELEC 펌웨어 기준으로 업로드되어 있습니다.
다른 펌웨어(OS)에서는 해당 OS에 맞는 위치에 복사하시면 사용가능합니다.


# 설치방법

/root 디렉토리안에 있는 파일들을 OGS 장비에 복사한다
    /root/storage 디렉토리는 /storage 에 디렉토리에 복사
    /root/tmo 디렉토리는 /tmp에 복사한다.

# 레트로아크에서 베젤 시스템 동작 방법

## 1. 게임별 베젤이 적용되는 순서는
   1) /storage/.config/retroarch/config/{ 코어 }/{ 게임시스템 }.cfg 설정 파일을 읽고
   2) /storage/roms/bezels/{ 게임시스템에서 지정한 파일명 }.cfg 베젤 설정 파일을 읽어 베젤을 표시한다.
   3) /storage/.config/retroarch/config/{ 코어 }/{ 게임시스템 }.glslp 
      세이더 프리셋 파일이 존재하면 세이더 설정파일을 읽어 세이더를 표시한다
      
~~참고) 
   코어별 지원하는 시스템은 retroarch 홈페이지에서 코어별 도큐먼트를 참고한다
   Nintendo Gameboy를 돌리는 코어인 Gambtte인 경우 
   https://docs.libretro.com/library/gambatte/ 페이지를 보면
   
   >
   > Emulated hardware (restart) [gambatte_gb_hwmode] (Auto|GB|GBC|GBA)
   >

   총 3개의 시스템을 지원한다고 되어 있다. 
   따라서 { 게임시스템 }.cfg는 해당 시스템 gb.cfg, gbc.cfg, gba.cfg 와 같이 작성하면 된다.
   Gameboy 베젤을 설정하여면 { 게임시스템 }.cfg는 gb.cfg가 된다.~~
   
## 2. 코어별 시스템 설정 파일
   코어별 설정 파일은 { 코어 } / { 코어 }.cfg 파일로 저장된다
   코어가 지원하는 시스템의 설정파일은 { 코어 } / { 게임시스템 }.cfg에 저장된다
   
   *.cfg 파일은 레크로아크 > 코어 > 시스템(에뮬레이터) > 게임개별 순서로 저장되며 게임개별 파일이 최우선으로 적용된다
   여기서 눈치빠른분은 아시겠지만 코어, 시스템, 개별게임 설정파일에 들어가는 내용은 레크로아크 설정파일 내용의 일부분이 되겠다. 레크로아크의 코어 문서(https://docs.libretro.com/)를 참조하거나 retroarch.cfg 파일을 열어서 참고해도 된다.

### 베젤을 적용하기 위한 코어.cfg 작성방법 

```
# Video 설정 - 해상도 설정
aspect_ratio_index = "21"
video_scale_integer = "true"
video_aspect_ratio_auto = "false"
```
**aspect_ratio_index** 는 화면 비율을 설정하는 항목이다. 값에 따라 화면 비율이 달라진다.
화면비율은 아래와 같은 값을 가진다
```
#   0 - 4:3   /  1 - 16:9 /  2 - 16:10 /  3 - 16:15 /  4 - 21:9  /  5 - 1:1
#   6 - 2:1   /  7 - 3:2  /  8 - 3:4   /  9 - 4:1   / 10 - 9:16  / 11 - 5:4
#  12 - 6:5   / 13 - 7:9  / 14 - 8:3   / 15 - 8:7   / 16 - 19:12 / 17 - 19:14
#  18 - 30:17 / 19 - 32:9
#  20 - Config - video_aspect_ratio 옵션 사용시
#  21 - 1:1 Par (10:9 DAR)
#  22 - Core Provided
#  23 - Custom - custom_viewport_... 옵션 사용시
```

**video_scale_integer**정수 배율로 스케일 조절을 할것인지 true/false를 정한다. true가 되면 2배, 3배 딱딱 떨어지는 화면비율이 만들어 진다. OGS는 이 항목이 true가 되면 원본 사이즈의 정확히 3배크기가 된다. 비율을 임의의 사이즈로 변경하려면 false가 되어야 한다.

**video_aspect_ratio_auto** 화면 비율을 변경하려면 이 항목은 무조건 false여야 한다. 레트로아크가 자동으로 조절하겠다는 항목이다


**- 화면위치나 사이즈를 임의로 변경하려면 aspect_ratio_index값을 23번 Custom으로 설정하고 아래 custom_viewport를 설정해야 한다.**

```
# Video 설정 - 사이즈 관련 - CUSTOM일때
custom_viewport_width = 480
custom_viewport_height = 432
custom_viewport_x = 187
custom_viewport_y = 25
```

### 베젤(오버레이) 설정
```
input_overlay = 베젤 이미지 정보를 가지고 있는 베젤 설정 파일위치 
input_overlay_opacity = 투명도
input_overlay_enable = "true" 무조건 true
input_overlay_scale = 1 무조건 1
input_overlay_enable_autopreferred = "true" 무조건 true
input_overlay_show_physical_inputs = "true" 무조건 true
input_overlay_hide_in_menu = "true" 무조건 true
```

**input_overlay** 항목은 베젤 이미지 정보를 가지고 있는 베젤 설정 파일위치를 지정한다. Full Path로 지정하는게 좋다
**input_overlay_opacity** = 투명도 1.0은 불투명이며 디버깅을 위해 0.9를 선호한다.
나머지 항목은 true로 그대로 사용한다.

## 세이더 설정

```
# Shaders
auto_shaders_enable = "true" 세이더를 사용하려면 무조건 true
video_shader_enable = "true" 세이더를 사용하려면 무조건 true
video_smooth = "false"  세이더를 사용하려면 무조건 false
```
세이더 설정은 할게 없다. 세이더를 사용하려면 **auto_shaders_enable** 항목만 true로 해주고 사용하지 않으려면 false를 기록한다.

**[ 중요 ]** 세이더 설정파일은 세이더 프리셋 파일(*.glslp)인데 아쉽게도 설정파일에서 세이더 파일을 지정해도 레크로아크에서 해당 세이더 프리셋 파일을 로딩을 하지 못하는 버그가 있다. 현재로는 **{ 코어 시스템명 }.glslp** 파일로 베젤설정파일과 같은 디렉토리에 세이더 프리셋 파일을 저장하면 된다. 또한 glslp 파일내 리소스 파일들의 경로를 자신의 시스템에 맞게 수정이 되어야 한다. 여기에 업로드 된것은 OGS의 EmuELEC 펌웨어를 기준으로 작성되어 있다.


### [예제] 아래는 게임기어 코어의 설정 파일이다

```
# =======================================
#            Gamegear 3X 베젤 설정 
# =======================================
#   0 - 4:3   /  1 - 16:9 /  2 - 16:10 /  3 - 16:15 /  4 - 21:9  /  5 - 1:1
#   6 - 2:1   /  7 - 3:2  /  8 - 3:4   /  9 - 4:1   / 10 - 9:16  / 11 - 5:4
#  12 - 6:5   / 13 - 7:9  / 14 - 8:3   / 15 - 8:7   / 16 - 19:12 / 17 - 19:14
#  18 - 30:17 / 19 - 32:9
#  20 - Config - video_aspect_ratio 옵션 사용시
#  21 - 1:1 Par (10:9 DAR)
#  22 - Core Provided
#  23 - Custom - custom_viewport_... 옵션 사용시

# Video 설정 - 사이즈 관련 - CUSTOM일때
custom_viewport_width = 480
custom_viewport_height = 432
custom_viewport_x = 187
custom_viewport_y = 25

# Video 설정 - 해상도 설정
aspect_ratio_index = "21"
video_scale_integer = "true"
video_aspect_ratio_auto = "false"

# Overlay - Bezel 설정
input_overlay = "/storage/roms/bezels/gamegear/default.cfg"
input_overlay_opacity = "0.900000"
input_overlay_enable = "true"
input_overlay_scale = 1
input_overlay_enable_autopreferred = "true"
input_overlay_show_physical_inputs = "true"
input_overlay_hide_in_menu = "true"

# Shaders
auto_shaders_enable = "true"
video_shader_enable = "true"
video_smooth = "false"
video_shader_dir = "/tmp/shaders/"
#video_shader = "/storage/roms/bezels/gb/default.glslp"

```

## 3. 베젤 설정 파일 작성방법

베젤 설정 파일은 /storage/roms/bezels 디렉토리에 저장되어 있다.
펌웨어 업데이트시 베젤 파일이 삭제되는 불상사를 방지하기 위한 위치 선정이다.

```
overlays = 1  
overlay0_overlay = default.png // cfg 파일과 같은 디렉토리에 베젤이미지를 저장한다
overlay0_full_screen = true
overlay0_descs = 0
```

**overlay0_overlay** 항목에 베젤 이미지 파일명을 넣는다. 보통 cfg파일과 같은 디렉토리에 넣기 때문에 경로없이 파일명만 적었는데 다른 위치에 있다면 Full Path로 입력한다

나머지 항목들은 예제 값 그대로 유지한다.

 
# 게임 적용 방법 (기본 EmuELEC)

[ 펌웨어 ] https://github.com/EmuELEC/EmuELEC/releases

OGS/EmuELEC 사용자는 /storage 아래에 있는 모든 파일을 장비에 /storage에 모두 복사하면 된다.
모든 파일을 복사하면 되지만 꼭 복사해야할 파일들은 아래와 같다.

예제) Nintendo Gameboy인 경우
>
>   1) /storage/.config/retroarch/config/Gambatte/**gb.cfg**
>   2) /storage/.config/retroarch/config/Gambatte/**gb.glslp**   // 세이더를 적용하고자 할때 같이 복사  
>   3) /storage/roms/bezels/**gb/default.cfg**
>   4) /storage/roms/bezels/**gb/default.png**
>

   - 적용될 베젤
   
![Gameboy 3x크기의 베젤 이미지](/root/storage/roms/bezels/gb/default.png)


# RetroArena 펌웨어 적용방법

[ 펌웨어 ] https://techtoytinker.com/odroid-go-advance

// TODO.



# 스크린샷

* Nintendo Gameboy

![gb 스크린샷](/screenshots/b-gb.png)


* Nintendo Gameboy Color

![gbc 스크린샷](/screenshots/b-gbc.png)


* Nintendo Gameboy Adcanced

![gba 스크린샷](/screenshots/b-gba.png)


* SNK NeoGeo Pocket Color

![ngpc 스크린샷](/screenshots/b-ngpc.png)


* SEGA Game Gear

![gb 스크린샷](/screenshots/b-gg.png)


* BANDAI WonderSwan

![wonderswan 스크린샷](/screenshots/b-wonderswan.png)


* BANDAI WonderSwan Color

![wonderswan color 스크린샷](/screenshots/b-wonderswancolor.png)





   
