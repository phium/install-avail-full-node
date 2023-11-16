**** kate체인에서 goldberg 체인으로 이미그레이션을 다루는 글입니다***


# avail 폴더로 이동
  `cd avail`
## kate 체인에서 업데이트시 데이터 폴더 삭제 이후 재 생성
`rm -rm data`
<br/>`mkdir -p data`
# service file 스탑
`sudo systemctl stop avail.service`
# 최신 버전 받아오기
`git pull`
# 실행
`cargo run --locked --release -- --chain goldberg -d ./output`
# 서비스 파일 수정
`sudo nano /etc/systemd/system/availd.service`

    [Unit] 
    Description=Avail Validator
    After=network.target
    StartLimitIntervalSec=0
    [Service] 
    User=$root 
    ExecStart= /root/avail/target/release/data-avail --base-path `pwd`/data --chain goldberg --name "$test"
    Restart=always 
    RestartSec=120
    [Install] 
    WantedBy=multi-user.target
# 실행
`sudo systemctl enable availd.service`
`sudo systemctl start availd.service`
