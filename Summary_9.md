# 차량용 OS 디바이스 프로그래밍

## 커널(Kernel)
>커널은 OS이다

### OS
- 선점 OS: 특정 프로세스가 CPU 를 독점하는것이 불가능(운영체제가 강제로 프로세스의 CPU 점유를 제어)하면 선점형

- 비선점 OS: 특정 프로세스가 CPU 를 독점하는것이 가능 (프로세스가 스스로 CPU 점유를 포기해야만 다른 프로세스가 실행)하면 비선점형


#### 디바이스 드라이버에서는 특정주소를 바로 접근하면 안된다.
- 디바이스 드라이버도 커널 영역에 속해있다.

>유닉스는 파일 시스템 구조에 크게 의존한다.
- Open, Read, Write, Close


make menuconfig

![](./picture/Linux_Device_Driver.png)


### 리눅스 컴파일 순서
1. make mds2450_deconfig
2. make zImage(linux kernel 실행파일)
3. rfs(root flie system)

nfs(network file system)를 사용하면 보드에 file system 이 있는 것처럼 사용 가능
리눅스 부팅 시 중요한 사항
- bootargs
- bootcmd

teraterm setting
```
bootdelay=3
baudrate=115200
ethaddr=00:40:5c:26:0a:5b
gatewayip=192.168.20.1
netmask=255.255.255.0
boot=test
serverip=192.168.20.90
ipaddr=192.168.20.11
bootcmd=tftp 30008000 zImage;bootm 30008000
bootargs=root=/dev/nfs rw nfsroot=192.168.20.90:/work/rootfs ip=192.168.20.246:192.168.20.90:192.168.20.1:255.255.255.0::eth0:off console=ttySAC1,115200n81
stdin=serial
stdout=serial
stderr=serial
```

static 함수 사용 : 한 파일 내에서만 사용하기 위해

zImage를 통해 만든 커널 버전과 동작시키려는 장치의 커널 버전이 동일해야 한다.

개발자에게 권고된 Major Number는 240-253

mknod [파일] type major minor

- 임베디드 리눅스에서 파일 실행이 되지 않는 이유 : 장치가 등록되어있지 않았기 때문


set /proc/devices

>데이터 관련 통신을 위해선 프로토콜 스펙을 꼭 봐야함

### ioctl
커널과 어플리케이션 간에 정보 교환시에 일반적인 data와 제어 data를 구분하기 위해 사용.
- 일반 데이터는 write
- 제어 데이터는 ioctl을 이용


>_IOW(magic number, index num, var)
```c
/*ioctl_myddrv.h*/
#ifndef _IOCTL_MYDRV_H_
#define _IOCTL_MYDRV_H_
#define IOCTL_MAGIC    254

typedef struct
{
	unsigned char data[26];	

} __attribute__ ((packed)) ioctl_buf;

#define IOCTL_MYDRV_TEST                _IO(  IOCTL_MAGIC, 0 )
#define IOCTL_MYDRV_READ                _IOR( IOCTL_MAGIC, 1 , ioctl_buf )
#define IOCTL_MYDRV_WRITE               _IOW( IOCTL_MAGIC, 2 , ioctl_buf )
#define IOCTL_MYDRV_WRITE_READ     _IOWR( IOCTL_MAGIC, 3 , ioctl_buf )

#define IOCTL_MAXNR                   4
#endif // _IOCTL_MYDRV_H_
```

### 프로세스 3대 핸들러
- signal handler
- interrupt handler
- timer handler


- 실시간으로 로그 확인하는 법 : tail -f /var/log/messages

### 리눅스 커널에서 타이머 작동 순서
```c
//1. timer_list structor 선언
struct timer_list timer;

void my_timer(unsigned long data)
{
    int i;
    for(i=1; i<=data; i++) {
    	printk("Kernel Timer Time-Out Function Doing_%d...\n", i);
    }

    //아래 2줄을 지우면 타이머는 1번만 작동함
    timer.expires = jiffies + 2*HZ;
    add_timer(&timer);

    printk("Kernel Timer Time-Out Function Done!!!\n");
}


int timerTest_init(void)
{
	printk(KERN_INFO "timerTest Module is Loaded!! ....\n");

    //2. timer initialization
    init_timer(&timer);

    //3.timer.expires = get_jiffies_64() + 3*HZ;
    timer.expires = jiffies + 3*HZ;
    
    timer.function = my_timer;
    timer.data = 5;
    add_timer(&timer);

    return 0;
}
```