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

## 디바이스 드라이버 I/O 읽기와 쓰기

- 어플리케이션의 read()는 커널로부터 정보를 읽어옴 - 커널의 read()는 어플리케이션에 정보를 write함(put_user, copy_to_user 함수)

- 어플리케이션의 write()는 커널에 정보를 보냄 - 커널의 write()는 어플리케이션의 정보를 read함(get_user, copy_from_user 함수)

- 사용자 모드에서 프로세스에 할당된 공간이 아닌 다른 공간에 접근하면 Segmentation Fault 오류 발생. - 커널 모드에서 할당된 공간이 아닌 다른 공간에 접근해도 오류가 안남

- 그러므로 Copy를 하여 잘못된 데이터 접근을 막도록 함

- Write()에서는 get_user()를 통해 어플리케이션으로부터의 데이터를 읽어온 후 write작업 실행

- Read()에서는 read작업을 실행 후 put_user()를 통해 어플리케이션에 데이터 보냄

- 디바이스 드라이버의 주번호는 32비트 중 상위 12비트, 부번호는 하위 20비트이다.

- 장치 파일을 통하여 사용자 공간과 커널 공간을 연결

![image](https://github.com/DonggeunC/Udoo-Device-Driver/assets/124149731/7ef4b3e4-9fb9-4c94-85f8-c8e43ba483b0)

## 디바이스 드라이버 타이머 인터럽트

- pdata->timer.expires = get_jiffies_64() + timeover; -> timeover는 100hz, jiffies는 헤르츠에 따라 쌓이는 전역변수 - 현재 시간에 100hz(1초)가 지난 순간 expire

- expire가 되면 timer.function의 함수 실행

- KERNEL_TIMER_MANAGER 구조체의 멤버변수인 led를 write하여 led가 켜지는 방식을 key입력으로 전환 가능

- p306 ioctl폴더의 dev.c에서 배열의 인덱스로 나열된 2진수 변환 -> for문 사용하여 값이 들어올 때 인덱스 i+1로 계산

- GFP_KERNEL옵션은 메모리를 항상 할당하도록 함 -> 메모리 할당할 공간이 없으면 프로세스가 sleep해버리는 단점이 있음

- 인터럽트 함수 매개변수가 대부분 void 포인터 -> void 포인터는 크기가 지정되지 않기 때문에 어떠한 자료형의 포인터도 저장할 수 있음

- 전역변수를 사용하지 않고 file 구조체의 포인터를 이용하여 함수 간 데이터를 이동시키도록 하여 key값을 읽고 irq값을 부여
