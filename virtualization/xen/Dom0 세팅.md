## 메모리 수정

/etc/default/grub에 config파일을 수정함으로써 Dom0을 수정할 수 있다
```
GRUB_TIMEOUT=5

GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"

GRUB_DEFAULT=saved

GRUB_DISABLE_SUBMENU=true

GRUB_TERMINAL_OUTPUT="console"

GRUB_CMDLINE_LINUX="rd.lvm.lv=fedora/root00 rd.lvm.lv=fedora/swap rhgb quiet"

GRUB_DISABLE_RECOVERY="true"

```
여기서 GRUB_CMDLINE_LINUX를 

GRUB_CMDLINE_LINUX="rd.lvm.lv=fedora/root00 rd.lvm.lv=fedora/swap rhgb quiet dom0_mem=512M,max:512M"

로 수정하면, dom0의 메모리 양을 수정할 수 있다.

그후
```
grub2-mkconfig -o /boot/grub2/grub.cfg

reboot
```
을하면 메모리양이 바뀐것을 확인 할 수 있다

[[balloning]]을 통해 실행중인 시스템의 설정도 수정할 수 있다



## VCPU 수정

/etc/default/grub 수정
```
  
GRUB_TIMEOUT=5

GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"

GRUB_DEFAULT=saved

GRUB_DISABLE_SUBMENU=true

GRUB_TERMINAL_OUTPUT="console"

GRUB_CMDLINE_LINUX="rd.lvm.lv=fedora/root00 rd.lvm.lv=fedora/swap rhgb quiet dom0_max_vcpus=2"

GRUB_DISABLE_RECOVERY="true"
```
여기서 dom_max_vcpus=2를 바꿈으로써 가상 cpu의 개수를 줄일 수 있다
그 뒤에 dom0_vcpus_pin 라는 문구를 추가하면, 항상 vcpu를 대기상태로 만들 수 있음(dom0에게)


## 다른 옵션
dom0_mem = X     dom0 에 할당 된 메모리
apic=auto                인터럽트 컨트롤러를 관리할 수 있다
noapic                      BIOS에서 승인되었어도 apic을 쓰지마라 
nosmp                      smp 비허가
watchdog                디버깅시 도움되고, 시스템 실패 시  도움이 됨
noreboot                  에러발생시 자동 재부팅 방지
VGA                          VGA 콘솔 쓰기
com1                         시리얼 포트 COM1사용
