1. 確認規則都進去了
(Invoke-RestMethod "http://localhost:9080/fhir/Library?_summary=count").total

要是 77。

(Invoke-RestMethod http://localhost:9080/fhir/Library/CRCReusable).name

有顯示名稱就代表內容正常。

((Invoke-RestMethod http://localhost:9080/fhir/metadata).rest[0].resource | Where-Object type -eq Library).operation.name

清單裡要有 evaluate，代表 CQL 運算功能有開。

2. 確認 CQL 審查 API 連到這台 FHIR Server

API 的設定（關鍵）：

Get-Content (Join-Path (Get-Website CQL_api).PhysicalPath appsettings.json) | Select-String "FHIRServerUrl|CancerDrupCodeFile"
FHIRServerUrl 要是 http://localhost:9080/fhir。如果是 172.31.6.201 這種舊 IP，要改掉，並重啟站台：Restart-WebItem 'IIS:\Sites\CQL_api'。
CancerDrupCodeFile 要指到實際存在的基因檢測項目 CSV 檔。

API 各功能有沒有回應：

Invoke-RestMethod http://localhost:5138/api/system/status

看系統狀態。裡面如果有 FHIR Server 的連線狀態，要顯示正常。

(Invoke-RestMethod http://localhost:5138/api/cql-rules) | Select-Object -First 5

規則清單要有資料。

(Invoke-RestMethod http://localhost:5138/api/drugs) | Select-Object -First 5

藥品清單要有資料。

Web：瀏覽器開 http://localhost:5139，看儀表板和規則清單有沒有正常顯示。
