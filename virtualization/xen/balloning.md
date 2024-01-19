
/etc/xen/xl.conf파일 수정
```
## Global XL config file ##

# Control whether dom0 is ballooned down when xen doesn't have enough
# free memory to create a domain. "auto" means only balloon if dom0
# starts with all the host's memory.

#autoballoon="auto"

# full path of the lockfile used by xl during domain creation
#lockfile="/var/lock/xl"

```
여기서 autoballon의 # 을 지우고 "auto"를 0으로 바꿔주면 xl이 임의로 dom0의 메모리를 수정할 수 없다

https://xenproject.org/2014/02/14/ballooning-rebooting-and-the-feature-youve-never-heard-of/ <-- 나중에 보고 채우기
