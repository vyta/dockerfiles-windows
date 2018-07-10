# escape=`
FROM microsoft/windowsservercore:1709 AS base

WORKDIR /fluentd

RUN powershell -Command "Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" 

# Fluentd depends on cool.io which is only available for Ruby < 2.5, so need to install Ruby < 2.5 and install msys2 to get dev tools
RUN choco install -y ruby --version 2.4.2.2 --params "'/InstallDir:C:\'" `
    && choco install -y msys2 --params "'/NoPath /NoUpdate /InstallDir:C:\ruby24\msys64'" `
    && refreshenv `
    && ridk install 2 3 `
    && gem install oj -v 3.6.3 --no-document `
    && gem install cool.io -v 1.5.3 --no-document `
    && gem install json -v 2.1.0 --no-document `
    && gem install fluentd -v 1.2.2 -i C:\fluentd --no-document

RUN powershell -Command "Remove-Item -Recurse -Force C:\ProgramData\chocolatey"

FROM microsoft/windowsservercore:1709
COPY --from=base ["C:\\fluentd", "C:\\fluentd"]

COPY fluent.conf /fluentd/conf/fluent.conf

ENTRYPOINT ["cmd", "/k", "C:\\fluentd\\bin\\fluentd", "-c", "C:\\fluentd\\conf\\fluent.conf"]
