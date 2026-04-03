# 🔌 Port Scanner

> **제작 목적**  
> 인프라 보안 진단의 가장 기초 단계인 **활성화 서비스 식별** 과정을 이해하고,  
> 불필요하게 열린 포트로 인한 **공격 표면(Attack Surface)을 분석**하기 위해 제작한 학습용 도구입니다.

---

## 📌 개요

인프라 보안 진단에서 포트 스캐닝은 대상 시스템에서 어떤 서비스가 외부에 노출되어 있는지 파악하는 첫 번째 단계입니다.  
불필요하게 열려 있는 포트는 공격자의 진입점이 될 수 있으며, 이를 사전에 식별하고 최소화하는 것이 공격 표면 축소의 핵심입니다.  
이 도구는 소켓 통신 원리부터 스레드 기반 병렬 스캔까지, 포트 스캐너의 동작 방식을 직접 구현하며 학습하기 위해 제작하였습니다.

---

## 🔍 스캔 방식

| 방식 | 설명 |
|---|---|
| **TCP Connect Scan** | 3-Way Handshake를 완전히 수행하여 포트 개방 여부 확인 (기본) |
| **Banner Grabbing** | 열린 포트에서 서비스 배너 정보를 수집하여 버전 식별 시도 |
| **Well-Known Port 매핑** | 포트 번호 기반 서비스명 자동 매핑 (HTTP, SSH, FTP 등) |

---

## 🚀 사용법

```bash
# 단일 호스트 기본 스캔 (Well-Known Ports: 1~1023)
python port_scanner.py -t 192.168.0.1

# 포트 범위 지정 스캔
python port_scanner.py -t 192.168.0.1 -p 1-65535

# 특정 포트만 스캔
python port_scanner.py -t 192.168.0.1 -p 22,80,443,3306,8080

# 스레드 수 조정 (기본값: 100)
python port_scanner.py -t 192.168.0.1 -p 1-10000 --threads 200

# 배너 정보 수집 포함
python port_scanner.py -t 192.168.0.1 --banner

# 결과 저장
python port_scanner.py -t 192.168.0.1 -o result.txt
```

---

## 📊 결과 예시

```
[+] Target  : 192.168.0.1
[+] Ports   : 1 - 1023
[+] Threads : 100
[+] Started : 2024-11-01 14:32:01
======================================
PORT      STATE    SERVICE       BANNER
--------------------------------------
22/tcp    OPEN     SSH           OpenSSH 8.9p1 Ubuntu
80/tcp    OPEN     HTTP          Apache/2.4.52 (Ubuntu)
443/tcp   OPEN     HTTPS         -
3306/tcp  OPEN     MySQL         5.5.5-10.6.12-MariaDB
8080/tcp  OPEN     HTTP-Proxy    -
======================================
[결과] 스캔 완료 | 소요 시간: 3.24초 | 열린 포트: 5개
```

---

## 💡 학습 포인트

- TCP/IP 3-Way Handshake 및 소켓 통신 원리 이해
- 스레드(Thread) 기반 병렬 처리로 스캔 성능 최적화
- Well-Known Port(0~1023), Registered Port(1024~49151) 구분 이해
- 공격 표면(Attack Surface) 개념 — 불필요한 포트 노출이 미치는 보안 영향 분석
- Nmap 등 전문 스캐너와의 동작 방식 비교를 통한 심화 학습

---

## ⚠️ 주의사항

> 이 도구는 **학습 목적**으로 직접 구현한 포트 스캐너입니다.  
> 포트 스캐닝은 대상 시스템의 소유자 허가 없이 수행할 경우  
> **정보통신망 이용촉진 및 정보보호 등에 관한 법률** 위반에 해당할 수 있습니다.  
> 반드시 **본인 소유 또는 명시적 허가를 받은 환경**(로컬 네트워크, 실습 환경 등)에서만 사용하십시오.

---

## 📚 참고 자료

- [RFC 793 - Transmission Control Protocol](https://www.rfc-editor.org/rfc/rfc793)
- [IANA Service Name and Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers)
- [Nmap 공식 문서 - Port Scanning Techniques](https://nmap.org/book/man-port-scanning-techniques.html)
- [OWASP Testing Guide - Network Infrastructure Testing](https://owasp.org/www-project-web-security-testing-guide/)
