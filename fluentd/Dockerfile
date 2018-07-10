# escape=`
FROM microsoft/windowsservercore:1709 AS base
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

WORKDIR /fluentd

RUN Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1')); `
    choco install -y ruby  --params "'/InstallDir:C:\Ruby'"; `
    Remove-Item -Recurse -Force C:\Ruby\msys64; `
    choco install -y msys2 --params "'/NoPath /NoUpdate /InstallDir:C:\Ruby\msys64'"; `
#    Update-SessionEnvironment; `
#    choco install -y ruby ruby2.devkit; `
    $env:Path += ';C:\Ruby\bin;'; `
    $gemrc_path = ${env:USERPROFILE} + '\gemrc'; `
    echo 'gem: --no-document' >> $gemrc_path; `
    gem update --system --no-document; `
    ridk install 2 3; `
#    ridk exec pacman -S mingw-w64-x86_64-sqlite3; `
    gem install oj -v 3.6.3; `
#    gem install coolio -v 1.5.3 --platform ruby; `
    gem install json -v 2.1.0; `
    gem install fluentd -v 1.2.2; 

#FROM microsoft/windowsservercore:1709
#COPY --from=base ["C:\\opt\\td-agent", "C:\\opt\\td-agent"]
#
#COPY fluent.conf /fluent/conf/fluent.conf

#ENTRYPOINT ["cmd", "/k", "C:\\opt\\td-agent\\embedded\\bin\\fluentd", "-c", "C:\\fluent\\conf\\fluent.conf"]
