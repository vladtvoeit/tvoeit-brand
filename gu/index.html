# GoldenUser - загрузчик для запуска одной строкой из PowerShell:
#
#     irm https://tvoe.it/gu | iex
#
# Живая копия этого файла лежит в репозитории tvoeit-brand как gu/index.html (GitHub Pages, tvoe.it).
# Загрузчик скачивает GoldenUser.ps1 строго указанной версии, сверяет SHA256 и запускает его
# от администратора из C:\Program Files\GoldenUser (обычные пользователи туда писать не могут).
# Новая версия: поставить метку в репозитории GoldenUser, поменять здесь $Version и $Sha256
# и скопировать файл в tvoeit-brand/gu/index.html.

& {
    $ErrorActionPreference = 'Stop'

    $Version = 'v1.1'
    $Sha256  = '5A42379F26A8610F002B5C99C923A2B7AE779E7446D5634CFBBBD83B2654B861'
    $Url     = "https://raw.githubusercontent.com/vladtvoeit/GoldenUser/$Version/GoldenUser.ps1"
    $Short   = 'https://tvoe.it/gu'

    try {
        # Старые сборки Windows 10 по умолчанию не включают TLS 1.2, без него GitHub не ответит.
        try { [Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12 } catch { }

        $isAdmin = ([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
        if (-not $isAdmin) {
            Write-Host 'GoldenUser: нужны права администратора, открываю PowerShell от имени администратора...'
            Start-Process powershell.exe -Verb RunAs -ArgumentList "-NoProfile -ExecutionPolicy Bypass -Command `"irm $Short | iex`""
            return
        }

        $dir    = Join-Path $env:ProgramFiles 'GoldenUser'
        $target = Join-Path $dir 'GoldenUser.ps1'
        $tmp    = Join-Path $env:TEMP ('GoldenUser_{0}.ps1' -f [guid]::NewGuid().ToString('N'))

        Write-Host "GoldenUser ${Version}: загрузка..."
        Invoke-WebRequest -Uri $Url -OutFile $tmp -UseBasicParsing
        $hash = (Get-FileHash -LiteralPath $tmp -Algorithm SHA256).Hash
        if ($hash -ne $Sha256) {
            Remove-Item -LiteralPath $tmp -Force -ErrorAction SilentlyContinue
            throw "Контрольная сумма не совпала (получено $hash). Файл не запущен."
        }

        if (-not (Test-Path -LiteralPath $dir)) { New-Item -ItemType Directory -Path $dir | Out-Null }
        Move-Item -LiteralPath $tmp -Destination $target -Force

        # Повторная проверка уже на месте запуска.
        if ((Get-FileHash -LiteralPath $target -Algorithm SHA256).Hash -ne $Sha256) { throw "Контрольная сумма $target не совпала. Файл не запущен." }

        Write-Host "GoldenUser ${Version}: проверено, запускаю $target"
        Write-Host 'Не закрывайте это окно, пока открыт GoldenUser: сюда выводятся ошибки запуска.'
        # Запуск в этой же консоли и с ожиданием: если скрипт упадёт до появления окна, текст ошибки останется здесь.
        & powershell.exe -NoProfile -ExecutionPolicy Bypass -STA -File $target
        if ($LASTEXITCODE) { throw "GoldenUser завершился с кодом $LASTEXITCODE - текст ошибки выше." }
    }
    catch {
        Write-Host "GoldenUser: ОШИБКА - $($_.Exception.Message)" -ForegroundColor Red
        Read-Host 'Нажмите Enter, чтобы закрыть' | Out-Null
    }
}
