# shellscript-for-Volatile-Data-Collection
Linux Volatile Data Collection. 리눅스 휘발성 정보 수집 쉘코드입니다.
Wazuh-Active-Response에서 사용했으며, rule.level에 도달했을 경우 자동으로 실행됩니다.
수집되는 범위는 다음과 같습니다.

----
**1. 시스템 정보**
- `uname -a` - 커널 및 시스템 정보
- `/etc/os-release`, `lsb_release -a` - OS 버전
- `uptime`, `who -b` - 시스템 가동 시간
- `date`, `timedatectl` - 시스템 시간
**2. 네트워크 연결 상태**

- `ss -antp` - TCP 소켓 연결
- `ss -uanp` - UDP 소켓 연결
- `lsof -nP -i` - 네트워크 연결 파일 디스크립터
- `ip -br a`, `ip r`, `ip neigh` - IP 주소, 라우팅, ARP
- `/etc/resolv.conf`, `/etc/hosts` - DNS 및 호스트 설정

**3. 프로세스 정보**

- `ps auxwwf` - 전체 프로세스 트리
- `pstree -p -a` - 프로세스 트리 상세
- `ps -eo pid,ppid,user,lstart,etime,cmd` - 프로세스 실행 시간 정보

**4. 의심 프로세스 상세 정보** 

- `/proc/[pid]/cmdline` - 명령행 인자
- `/proc/[pid]/exe` - 실행 파일 경로
- `/proc/[pid]/cwd` - 작업 디렉토리
- `/proc/[pid]/status` - 프로세스 상태
- `/proc/[pid]/environ` - 환경 변수
- `/proc/[pid]/fd` - 파일 디스크립터 목록
- `lsof -p [pid]` - 해당 PID가 사용하는 파일/네트워크

**5. 로그인 및 사용자 정보**

- `w` - 현재 로그인 사용자 및 작업
- `who -a` - 로그인 사용자 상세
- `last -n 50` - 최근 50개 로그인 기록

**6. 시스템 로그—> 주석처리 해둠**

- `/var/log/auth.log` (최근 2000줄) - 인증 로그
- `/var/log/syslog` (최근 2000줄) - 시스템 로그
- `/var/log/kern.log` (최근 2000줄) - 커널 로그
- `journalctl --since '30 minutes ago'` - 최근 30분 저널 로그
- `/var/log/falco/falco.log` - Falco 로그 (있는 경우)

**7. 명령어 히스토리**

- `/root/.bash_history` - root 명령어 히스토리
- `/home/*/.bash_history` - 모든 사용자 명령어 히스토리

**8. 메타데이터**

- incident_id, timestamp, hostname
- rule_id, trigger_user, source_ip
- alert JSON (Wazuh에서 제공하는 경우)
