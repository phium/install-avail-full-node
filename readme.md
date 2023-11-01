AVAIL 풀 노드 셋업

공식링크 : https://docs.availproject.org/operate/node/binaries/

**사양**

최소：4GB RAM，2코어CPU，20-40GB SSD

권장：8GB RAM，4코어CPU，200-300GB SSD

Ubuntu 22.04 

#####시스템 설정

    sudo apt-get update
    sudo apt install build-essential
    sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    source ~/.cargo/env
    rustup default stable
    rustup update
    rustup update nightly
    rustup target add wasm32-unknown-unknown --toolchain nightly

**### avail 프로젝트 레포 복사 & 테스트**
향후 체인 이름 변경시 $체인 이름 변경



    git clone https://github.com/availproject/avail.git
    cd avail
    mkdir -p output
    mkdir -p data
    git checkout v1.7.2
    cargo run --locked --release -- --chain $kate -d ./output

**##kate체인으로 연결시##**

    2023-11-01 16:11:31 Avail Node    
    2023-11-01 16:11:31 ✌️  version 1.7.0-ad024ff050e    
    2023-11-11 16:11:31 ❤️  by Anonymous, 2017-2023    
    2023-11-11 16:11:31 📋 Chain specification: Avail Kate Testnet    
    2023-11-11 16:11:31 🏷  Node name: test    
    2023-11-11 16:11:31 👤 Role: FULL    
    2023-11-11 16:11:31 💾 Database: RocksDb at /tmp/substrateJwM8xd/chains/Avail Testnet_116d7474-0481-11ee-bc2a-7bfc086be54e/db/full    


**화면을 나온후 설정**

**Systemd 생성및 설정**

    sudo touch /etc/systemd/system/availd.service
    sudo vim /etc/systemd/system/availd.service

**service파일 설정 및 저장**

    [Unit] 
    Description=Avail Validator
    After=network.target
    StartLimitIntervalSec=0
    [Service] 
    User=$root 
    ExecStart= /root/avail/target/release/data-avail --base-path `pwd`/data --chain kate --name "$test"
    Restart=always 
    RestartSec=120
    [Install] 
    WantedBy=multi-user.target




**service 파일 enable 및 실행：

    sudo systemctl enable availd.service

    sudo systemctl start availd.service

**상태 확인

    sudo systemctl status availd.service

**혹은 journalctl 명령어로 확인 가능

    journalctl -f -u availd

**이후 노드 확인 
https://telemetry.avail.tools/

