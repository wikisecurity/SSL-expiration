    명령어
    ex) python ssl-check.py -nb -c -e j.nam12@wikisecurity.net -f hosts-sample.txt

    python ssl-check.py [옵션] [이메일] -f hosts-sample.txt
    
hosts-sample.txt에 SSL 인증스캔할 주소
현재: 
# 목록
    ; This is comment
    wiki.wikisecurity.net
    wikisecurity.net
    rura.wikisecurity.net
    ms.wikisecurity.net
    rav.wikisecurity.net
    shodanapi.wikisecurity.net
    vul.wikisecurity.net
    vul-api.wikisecurity.net
    ws.wikisecurity.net
    threat.wikisecurity.net
    bugbounty.wikisecurity.net
    mp4cu.wikisecurity.net
    bugbp.wikisecurity.net
    marshal.wikisecurity.net
    api-bugbp.wikisecurity.net
    api-marshal.wikisecurity.net
    api-file.wikisecurity.net
    api-mp4cu.wikisecurity.net
    opjt.wikisecurity.net


# 설치필요
    $ pip install -U colorama
    $ pip install -U dnspython
    $ pip install -U requests
    $ pip install -U PySocks

파이선 3.6버전 이상

# 옵션 명령어

    Options:
      -h, --help            Help
      -v, --version         Display the version number
      -f FILE, --file FILE  Path to the file with the list of hosts (default is None)
                            Sample:
                                    MyHosts.txt
      -o STRING, --host STRING
                            Host to check the expiration date of the ssl certificate (default is None)
                            Sample:
                                    google.com
      -c, --print-to-console
                            Enable console printing (default is False)
      -dw DAYS, --days-to-warn DAYS
                            Warn me when there are less than DAYS days left (default is 7)
      -t, --use-telegram    Send a warning message through the Telegram (default is False)
      -p URL, --proxy URL   Proxy link (for Telegram only, default is None)
                            Sample (for example, when the Tor browser is running):
                                    socks5://127.0.0.1:9150
      -e EMAIL or EMAIL'S, --email-to EMAIL or EMAIL'S
                            Send a warning message to email address (default is None)
                            Sample:
                                    email@mail.dot
                            Or multiple address (the addresses are separated by commas,
                            and the entire string is enclosed in double quotes):
                                    "email1@mail.dot, email2@mail.dot, email3@mail.dot"
      -subject STRING, --email-subject STRING
                            Append custom text to the email subject (default is None)
      -ssl, --email-ssl     Send email via SSL (default is False)
      -auth, --email-auth   Send email via authenticated SMTP (default is False)
      -starttls, --email-starttls
                            Send email via STARTTLS (default is False)
      -g, --use-google-dns  Use Google DNS server 8.8.8.8 for resolve hosts (default is False)
      -nb, --no-banner      Do not print banner (default is False)
    

 

# 결과:

![image](https://github.com/wikisecurity/SSL-expiration/assets/122947746/e81c0128-ef39-4b59-a032-86d434fd1ff9)




-------

# 수정해야할 부분

SMTP 연동문제로  결과값 나오나 알람메일이 보내지지 않음

![image](https://github.com/wikisecurity/SSL-expiration/assets/122947746/399a9941-83dd-46df-8f25-da05ffd4b27f)



코드

    # SMTP options
    SMTP_SERVER: str = os.getenv("SMTP_SERVER", "smtp.gmail.com")
    SMTP_PORT: int = int(os.getenv("SMTP_PORT", "587"))
    
    # SMTP_SERVER: str = os.getenv("SMTP_SERVER", "smtp.gmail.com")
    # SMTP_PORT: int = int(os.getenv("SMTP_PORT", "587"))  # For starttls
    
    # SMTP_SERVER: str = os.getenv("SMTP_SERVER", "smtp.mail.ru")
    # SMTP_PORT: int = int(os.getenv("SMTP_PORT", "25"))  # Default
    
    # SMTP_SERVER: str = os.getenv("SMTP_SERVER", "smtp.yandex.ru")
    # SMTP_PORT: int = int(os.getenv("SMTP_PORT", "465"))  # For SSL
    
    SMTP_SENDER: str = os.getenv("SMTP_SENDER", "j.nam12@wikisecurity.net")
    SMTP_PASSWORD: str = os.getenv("SMTP_PASSWORD", "zgrw hjki jbuu zyis")
    SMTP_CHECK_SSL_HOSTNAME: bool = False if str(os.getenv("SMTP_CHECK_SSL_HOSTNAME")) == "0" else True


    Gmail에서도 설정이 필요해보임


# 잔여일 알람 설정


line 793부터 831까지

코드

     dt_now = datetime.now()
            sdt = data.get('notAfter')
            dt = datetime.strptime(sdt, '%b %d %H:%M:%S %Y GMT')
            dt_delta = dt - dt_now
    
            is_expiried = dt_delta.days <= CLI.days_to_warn
            is_expiried_warn = dt_delta.days <= CLI.days_to_warn + 7
            if is_expiried:
                row_color = f'{FRC}'
                row_color2 = f'{FLR}'
            elif is_expiried_warn:
                row_color = f'{FY}'
                row_color2 = f'{FLY}'
            else:
                row_color = f'{FC}'
                row_color2 = f'{FLC}'
    
            if is_expiried:
                expires_hosts.append(f'{current_host.lower()};{dt_delta.days}')
    
            s_host = current_host.lower().rjust(max_len_hostname + 8)
            s_days = '    ' + str(dt_delta.days)
            s_days = s_days.ljust(10)
            print(
                f'{row_color}{s_host}',
                f'{row_color2}{s_days}',
                f'{FLBC}{country_name}, {organization_name}, {common_name}'
            )
    
        if len(expires_hosts) > 0:
            if CLI.use_telegram:
                res = send_expires_telegram(expires_hosts)
                if res is not None:
                    if res.status_code != 200:
                        print(f'{FLR}{res.text}')
    
            if CLI.email_to:
                send_expires_email(expires_hosts)
        print(f'{SR}')
      
