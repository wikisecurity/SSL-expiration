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






  
