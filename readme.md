# PlatformIO template for Clangd
このtemplate通りにやると、PlatformIO CLIでClangdを使ったコード補完ができるようになります。targetにesp32を指定していますが、ArduinoやSTM32等のマイコンでもできると思います。  

# compile_commands.jsonって何？
[compile_commands.jsonの参考](https://uchan.hateblo.jp/entry/2018/12/29/104132)  
私はNeoVimでのコード補完のために使用しています。  

PlatformIO CLIではcompile_commands.jsonを自動生成することができるため、今回は自動生成されるcompile_commands.jsonを使用します。  
[自動生成方法](https://docs.platformio.org/en/latest/integration/compile_commands.html)

# requirement
- PlatformIO CLI
  - [PlatformIO CLIのインストール方法](https://docs.platformio.org/en/latest/core/installation/methods/installer-script.html)
- option
  - make
  - minicom
```
mkdir project-name
cd project-name
# プロジェクトを作成
pio init --board=esp32dev
# src/main.cppを作成
# 本来は自動で生成されるはずだが、私の環境では作成されなかったため、githubからコピー
curl -s https://raw.githubusercontent.com/yudai0804/platformio-template-clangd/master/src/main.cpp > path/to/project-name/src/main.cpp
# extra_script.pyをプロジェクトにコピー
curl -s https://raw.githubusercontent.com/yudai0804/platformio-template-clangd/master/extra_script.py > path/to/project-name/extra_script.py
# platformio.iniの末尾にextra_scripts = pre:extra_script.pyを追加
echo extra_scripts = pre:extra_script.py >> platformio.ini
# .gitignoreの末尾にcompile_commands.jsonを追加
echo compile_commands.json >> .gitignore
```

**option**  
以下のmakefileも導入しておくと便利です  
```
curl -s https://raw.githubusercontent.com/yudai0804/platformio-template-clangd/master/Makefile > path/to/project-name/Makefile

```
## compile_commands.jsonを生成
```
pio run -t compiledb
cp .pio/build/esp32dev/compile_commands.json .
```
or  
```
make clangd
```

compile_commands.jsonを生成すればclangdによるコード補完が機能するはずです  

## ビルド
```
pio run
```
or  
```
make build
```

## 書き込み
書き込み前にビルドがされていなかった場合は、ビルドが自動に行われてから書き込まれます  
```
pio run --target upload
```
or  
```
make upload
```

## clean
```
pio run --target clean
```
or  
```
make clean
```

## minicomによるシリアル通信(option)
minicomを使用している場合は`make serial`でシリアル通信を開始できます
