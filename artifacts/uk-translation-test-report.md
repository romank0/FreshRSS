# Ukrainian Translation Validation Report

*2026-04-13T10:20:37Z by Showboat 0.6.1*
<!-- showboat-id: 0f6cd401-4f25-4af7-8b40-7926a458cb27 -->

This report validates Ukrainian translation updates using repository checks and browser-level verification with Rodney.

```bash
if rg -n '// TODO' app/i18n/uk; then echo 'Found untranslated TODO markers'; exit 1; else echo 'No TODO markers remain in app/i18n/uk'; fi
```

```output
No TODO markers remain in app/i18n/uk
```

```bash
composer run-script translations
```

```output
> cli/manipulate.translation.php --action format && php cli/compile.plurals.php --all && cli/check.translation.php --generate-readme
Compiled 0 plural file(s).
Successfully written translation status into README.md
Successfully written translation status into README.fr.md
```

```bash
set -euo pipefail
php -S 127.0.0.1:8081 -t p >/tmp/freshrss-php-server.log 2>&1 &
server_pid=$!
trap 'kill $server_pid >/dev/null 2>&1 || true; uvx rodney stop --local >/dev/null 2>&1 || true' EXIT
sleep 2
uvx rodney start --local
uvx rodney open 'http://127.0.0.1:8081/i/?step=0'
uvx rodney select '#language' 'uk'
uvx rodney click 'button[type=submit]'
uvx rodney wait 'body'
uvx rodney assert "document.body.innerText.includes('рекомендована бібліотека php-intl')"
uvx rodney assert "document.body.innerText.includes('Налаштування бази даних')"
uvx rodney stop --local
kill $server_pid >/dev/null 2>&1 || true
wait $server_pid 2>/dev/null || true
trap - EXIT

```

```output
Chrome started (PID 10122)
Debug URL: ws://127.0.0.1:33353/devtools/browser/e8f11c59-ff53-4087-a49a-4db9ceea5991
Installation · FreshRSS: step 1
Selected: uk
Clicked
Element visible
pass
pass
Chrome stopped
```
