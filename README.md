$dir  = "D:\CQL_lib"
$base = "http://localhost:9080/fhir"
$ok = 0; $fail = @()
Get-ChildItem $dir -Filter "Library-*.json" -Recurse | ForEach-Object {
    $bytes = [IO.File]::ReadAllBytes($_.FullName)
    $id = ([Text.Encoding]::UTF8.GetString($bytes) | ConvertFrom-Json).id
    try {
        Invoke-RestMethod -Method Put -Uri "$base/Library/$id" -Body $bytes -ContentType "application/fhir+json; charset=utf-8" | Out-Null
        $ok++
    } catch { $fail += "$id : $($_.Exception.Message)" }
}
"成功 $ok 筆，失敗 $($fail.Count) 筆"
$fail
