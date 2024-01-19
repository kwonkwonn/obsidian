[[LVM]]참고

```
cfdisk 
혹은
fdisk -l
```
로 현재 메모리 양과 가상화 된 메모리 양을 알 수 있다.
![](https://i.imgur.com/HnRxSEv.png)
이런 식으로 뜬다고 함

![](https://i.imgur.com/aOUTjd4.png)
이건 cfdisk를 입력했을 때.

리눅스 파일 시스템에서 /dev/파일 내부에 이런 드라이브들이 저장되어 있는데, 
"hd" 하드디스크 "sd"는 SCSI라고 한다
예를 들어 hda1이라는 파일이 있다면 이는 
hd- 하드디스크
a- 표기되는 첫번째 하드디스크 
1-첫번째 우선 파티션 이라는 뜻이다.  파티션은 초기에는 1...4사이의 숫자로 우선 분리되고, 논리 파티션은 5부터 시작한다



# 새 파티션을 추가하는 법

	cfdisk /dev/sdb 
커맨드를 통해서 쉽게 추가 된 하드디스크에서 파티션을 만들 수 있다 

그러나 parted라는 툴을 통해서 미리 초기화를 하고 만드는것을 추천함. 

	parted /dev/sdb mklabel msdos
	parted /dev/sdb mkpart primary
그 후 몇가지 선택사항을 완료하면 파티션을 나눠준다.

	fdisk /dev/sdb/ -l 
![](https://i.imgur.com/sFHibSD.png)

	parted -s /dev/sdb set 1 lvm on
	위와 같은 커맨드로 sdb에 파티션1번을 만들거나

	  fdisk /dev/sdb -l  
![](https://i.imgur.com/lvk2xPW.png)
뒤에 LVM이 따라온걸 봐서 LVM이 실행 됐다는걸 알 수 있음

아래의 커맨드를 고려
```
pvcreate /dev/sdb1
vgcreate (group name) /dev/sdb1 -v
lvcreate  -L (size(ex.8G)) -v -n (volume name) (group name)
```
![](https://i.imgur.com/8F5UmMN.png)
결과 

![](https://i.imgur.com/jSDh7BB.png)
