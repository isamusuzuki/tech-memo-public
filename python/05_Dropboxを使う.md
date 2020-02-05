# Dropbox を使う

作成日 2019/11/20

## 01. 指定したフォルダにあるファイル名を取得する

```python
import os

import dropbox

DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')
FOLDER_NAME = 't00000-atc1222'

dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)

res = dbx.files_list_folder(f'//Everglow/7.画像処理対象/{FOLDER_NAME}/color/')

for entry in res.entries:
    if '.jpg' in entry.name or '.png' in entry.name:
        print(f'name={entry.name}, id={entry.id}')

print('done')
```

## 02. 指定したファイルをダウンロードする

```python
import os

import dropbox

subfolder = 'temp/1_original'
if not os.path.exists(subfolder):
    os.mkdir(subfolder)

DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')
FOLDER_NAME = 't00000-atc1222'

dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)

res = dbx.files_list_folder(f'//Everglow/7.画像処理対象/{FOLDER_NAME}/color/')

for entry in res.entries:
    if '.jpg' in entry.name or '.png' in entry.name:
        local_path = f'{subfolder}/{entry.name}'
        dropbox_path = entry.id
        meta = dbx.files_download_to_file(local_path, dropbox_path)
        print(meta.name)

print('done')
```

## 03. 指定したファイルをアップロードする

```python
import os
import subprocess

import dropbox

subfolder1 = 'temp/1_original'
subfolder2 = 'temp/2_cropped'
targets = ['c1.jpg', 'c2.jpg', 'c3.jpg',
           'c4.jpg', 'c5.jpg', 'c6.jpg', 'c7.jpg']

if not os.path.exists(subfolder2):
    os.mkdir(subfolder2)

# ImageMagick: 元画像を自動で切り抜きする
for target in targets:
    cmd = (
        f'magick {subfolder1}/{target} '
        '-fuzz 0% -trim '
        f'{subfolder2}/{target}'
    )
    proc = subprocess.Popen(cmd, shell=True)
    print(f'process {proc.pid} starts')
    proc.wait()


DROPBOX_ACCESS_TOKEN = os.environ.get('DROPBOX_ACCESS_TOKEN')
FOLDER_NAME = 't00000-atc1222'

dbx = dropbox.Dropbox(DROPBOX_ACCESS_TOKEN)

for target in targets:
    with open(f'{subfolder2}/{target}', 'rb') as f:
        content = f.read()
        path = f'/Everglow/7.画像処理対象/{FOLDER_NAME}/処理結果/{target}'
        meta = dbx.files_upload(content, path, dropbox.files.WriteMode.add)
        print(meta.id)

print('done')
```
