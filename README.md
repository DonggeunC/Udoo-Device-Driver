# Udoo-Device-Driver

## 디바이스 드라이버 모듈 C파일

- printk함수를 이용하여 커널에 메세지 출력

- gpio에 접근하여 M3보드의 LED 제어

- mknod(device이름, device 타입(char나 block), 주번호, 부번호) 함수를 사용하여 디바이스를 만들고 시작
