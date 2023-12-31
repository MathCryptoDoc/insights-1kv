# insights-1kv

## Overview
Insights into the Polkadot/Kusama 1kv programme.

Public version: [insights.math-crypto.com](https://insights.math-crypto.com/)

The website is static and built with [doks](https://getdoks.org/), whose source files are generated by a python backend. 
The backend stores its data in dataframes that are easily loaded in, for example, notebooks.
Uploading of the website is done with [rclone](https://rclone.org/) but can be easily changed since we host a static website.

## Backend Python installation and deployment

### Install deps

``pip3 install urllib3 certifi idna matplotlib pandas paramiko pyarrow joblib``

Run ``cd scripts; ./make_dirs.sh``

### Test 
```
cd python
./active_eras.py
./dump-1kv-data.py
./make_score_figures.py
./generate_web_pages.py
```
Use notebooks to inspect dataframes.

### Deploy with service files

```
cd services

sudo cp 1kv-dump-json.service /etc/systemd/system/
sudo systemctl start 1kv-dump-json.service
sudo systemctl status 1kv-dump-json.service
sudo journalctl -u 1kv-dump-json --since today -f
sudo systemctl enable 1kv-dump-json.service

sudo cp 1kv-dump-chain.service /etc/systemd/system/
sudo systemctl start 1kv-dump-chain.service
sudo systemctl status 1kv-dump-chain.service
sudo journalctl -u 1kv-dump-chain --since today -f
sudo systemctl enable 1kv-dump-chain.service

sudo cp 1kv-score.service /etc/systemd/system/
sudo systemctl start 1kv-score.service
sudo systemctl status 1kv-score.service
sudo journalctl -u 1kv-score --since today -f
sudo systemctl enable 1kv-score.service

sudo cp 1kv-website.service /etc/systemd/system/
sudo systemctl start 1kv-website.service
sudo systemctl status 1kv-website.service
sudo journalctl -u 1kv-website.service --since today -f
sudo systemctl enable 1kv-website.service
```


## Doks website

The ``doks`` directory is a clone of main doks github at end of 2021. Custom files are in ``doks/content`` and (unfortunately) in many other places.

### Install deps

Install nvm on github locally as in https://github.com/nvm-sh/nvm
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
# log out & in
cd doks
nvm install node
nvm install-latest-npm
npm install
```
Hugo is in ``./node_modules/.bin/hugo/hugo``

# Test site locally

```
npm run start
```

## Build

```
npm run build
```

Files are in directory public.
