# Udoo-Device-Driver

## 디바이스 드라이버 모듈 C파일

- printk함수를 이용하여 커널에 메세지 출력

- gpio에 접근하여 M3보드의 LED 제어

- mknod(device이름, device 타입(char나 block), 주번호, 부번호) 함수를 사용하여 디바이스를 만들고 시작

- 하위 20비트는 부번호(MINOR), 상위 12비트는 주번호(MAJOR) 합쳐서 32비트

- No such device or address 오류 -> 디바이스 파일은 있으나 디바이스 드라이버를 적재하지않음. 

- No such file or directory 오류 -> 디바이스 드라이버는 적재했으나 디바이스 파일이 존재하지 않음 
