# Udoo-Device-Driver

## 디바이스 드라이버 모듈 C파일

- printk함수를 이용하여 커널에 메세지 출력

- gpio에 접근하여 M3보드의 LED 제어

- mknod(device이름, device 타입(char나 block), 주번호, 부번호) 함수를 사용하여 디바이스를 만들고 시작

- 하위 20비트는 부번호(MINOR), 상위 12비트는 주번호(MAJOR) 합쳐서 32비트

- No such device or address 오류 -> 디바이스 파일은 있으나 디바이스 드라이버를 적재하지않음. 

- No such file or directory 오류 -> 디바이스 드라이버는 적재했으나 디바이스 파일이 존재하지 않음 

- p184에서 디바이스 드라이버인 dev와 어플리케이션인 app을 만들어 dev를 적재하고 app을 통해 제어할 수 있도록 함

- p184_led에서 led_app argv[1]을 이용하여 led를 제어하고 그 상태를 출력하도록 코드를 작성

- led_app.c에서 read나 write함수를 이용하여 매개변수 char의 포인터형으로 보내는데 이 부분을 이용하여 led를 제어하고자 한다면 led_dev.c에서 led_write의 매개변수에 *buf 를 사용해주어야 한다

- 어플리케이션의 read()는 커널로부터 정보를 읽어옴 - 커널의 read()는 어플리케이션에 정보를 write함(put_user, copy_to_user 함수)

- 어플리케이션의 write()는 커널에 정보를 보냄 - 커널의 write()는 어플리케이션의 정보를 read함(get_user, copy_from_user 함수)
