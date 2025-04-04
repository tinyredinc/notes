# Open WebUI Deployment

## Conda Environment
1. Install Anaconda3
    ```
    AnacondaRepo: https://repo.anaconda.com/archive/

    wget -O anaconda.sh https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh

    bash anaconda.sh
    ```

2. Common CMD
    - Get general info of conda
        ```
        conda info
        ```
    - List all env
        ```
        conda env list
        ```
    - Active env
        ```
        conda activate {env_name}
        ```
        ```
        conda activate open-webui
        ```
    - Deactivate env
        ```
        conda deactivate
        ```
        ```
        conda deactivate {env_name}
        ```
    - List packages in env
        ```
        conda list
        ```
        ```
        conda list -n {env_name}
        ```
        ```
        conda list --explicit
        ```
    - Create env
        ```
        conda create -n {env_name} python={version_num}
        ```
        ```
        conda create --name {env_name} --clone {env_name}
        ```
    - Remove env
        ```
        conda remove --name {env_name} --all
        ```
    - Export and import
        - packages
        ```
        conda list --explicit > {spec-file.txt}
        ```
        ```
        conda install --name {env_name} --file {spec-file.txt}
        ```
        - environment
        ```
        conda env export > {environment_file}.yml
        ```
        ```
        conda env create -f {environment_file}.yml
        ```
    - ENV Variable
        ```
        cd $CONDA_PREFIX
        mkdir -p ./etc/conda/activate.d
        mkdir -p ./etc/conda/deactivate.d
        touch ./etc/conda/activate.d/env_vars.sh
        touch ./etc/conda/deactivate.d/env_vars.sh

        vim ./etc/conda/activate.d/env_vars.sh
        #!/bin/sh
        export MY_KEY='secret-key-value'
        export MY_FILE=/path/to/my/file/

        vim ./etc/conda/deactivate.d/env_vars.sh
        #!/bin/sh
        unset MY_KEY
        unset MY_FILE
        ```


## Open-Webui Setup

1. Create Conda Env for Open WebUI
```
conda create -n open-webui python=3.11
conda activate open-webui
```

2. Install open-webui via pip
```
pip install open-webui
```

3. Update open-webui via pip
```
pip install -U open-webui
```

4. Start open-webui
```
open-webui serve
```
5. open-webui.service
- sudo vim /home/red/anaconda3/envs/open-webui/start-open-webui.sh
```
#!/bin/bash
conda activate open-webui
exec open-webui serve
```
```
chmod +x /home/red/anaconda3/envs/open-webui/start-open-webui.sh
```
- sudo vim /etc/systemd/system/open-webui.service
```
[Unit]
Description=OpenWebUI Service
After=network.target

[Service]
Environment="PATH=/home/red/anaconda3/bin:/home/red/anaconda3/envs/open-webui/bin:$PATH"
WorkingDirectory=/home/red/anaconda3/envs/open-webui
User=red
Group=red
ExecStart=/home/red/anaconda3/envs/open-webui/start-open-webui.sh
Restart=on-failure
RestartSec=5s

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
sudo systemctl enable open-webui
sudo systemctl start open-webui

journalctl -u open-webui -f

sudo systemctl restart open-webui
sudo systemctl stop open-webui
```
