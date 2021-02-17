# 시작하기전에
이 베젤 파일은 하드커널사의 ODROID GO SUPER 장비에서 EmuELEC 펌웨어 사용자를 위한 파일입니다

# 설치방법

/root 디렉토리안에 있는 파일들을 OGS 장비에 복사한다
    /root/storage 디렉토리는 /storage 에 디렉토리에 복사
    /root/tmo 디렉토리는 /tmp에 복사한다.
    
# 게임 적용 방법

1. 게임별 베젤이 적용되는 순서는
   1) /storage/.config/retroarch/config/{ 코어 }/{ 게임시스템 }.cfg 설정 파일을 읽고
   2) /storage/roms/bezels/{ 게임시스템에서 지정한 파일명 }.cfg 베젤 설정 파일을 읽어 베젤을 표시한다.
   3) /storage/.config/retroarch/config/{ 코어 }/{ 게임시스템 }.glslp 
      세이더 프리셋 파일이 존재하면 세이더 설정파일을 읽어 세이더를 표시한다
      
참고) 1)에서 코어별 지원하는 시스템은 retroarch 홈페이지에서 코어별 도큐먼트를 참고한다
   Nintendo Gameboy를 돌리는 코어인 Gambtte인 경우 
   https://docs.libretro.com/library/gambatte/ 페이지를 보면
   
   >
   > Emulated hardware (restart) [gambatte_gb_hwmode] (Auto|GB|GBC|GBA)
   >

   총 3개의 시스템을 지원한다고 되어 있다. 
   따라서 { 게임시스템 }.cfg는 해당 시스템 gb.cfg, gbc.cfg, gba.cfg 와 같이 작성하면 된다.
   Gameboy 베젤을 설정하여면 { 게임시스템 }.cfg는 gb.cfg가 된다.
   

예제) Nintendo Gameboy인 경우
>
>   1) /storage/.config/retroarch/config/Gambatte/**gb.cfg**
>   2) /storage/.config/retroarch/config/Gambatte/**gb.glslp**   // 세이더를 적용하고자 할때 같이 복사  
>   3) /storage/roms/bezels/**gb/default.cfg**
>   4) /storage/roms/bezels/**gb/default.png**
>

   - 적용될 베젤
   
![Gameboy 3x크기의 베젤 이미지](/root/storage/roms/bezels/gb/default.png)



   
