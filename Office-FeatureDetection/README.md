Office - Feature Detection
===

Detekce, zda add-in běží v aplikaci Office a selektivní načtení Office.js:

```
@if (Request.QueryString.GetValues("_host_info") != null)
{
    <script src="https://appsforoffice.microsoft.com/lib/1/hosted/office.js" type="text/javascript"></script>
    <script src="~/Scripts/ExcelApp.js" type="text/javascript"></script>
}
```

**_host_info** obsahuje informaci o typu klienta, kde aplikace běží, např. *_host_Info=Excel|Web|16.00|en-US|ed67539d-0516-3a3c-103d-1788ee84ce81*

Pro rozpad na hodnoty můžeme použít šikovnou funkci:

```javascript
function getHostInfo() {
    var hostInfoValue = sessionStorage.getItem('hostInfoValue');
 
    // Parse the value string (reference: office.debug.js)
    var items = hostInfoValue.split('$');
    if (!items[2]) {
        items = hostInfoValue.split('|');
    }
 
    var hostInfo = {
        type: items[0],
        platform: items[1],
        version: items[2],
        //culture: items[3] // Some platforms (i.e. Win32) returns a culture property
    }
    return hostInfo;
}
```

> Autorem je Simon Jaeger: http://simonjaeger.com/where-am-i-detecting-the-office-host-in-office-add-ins/

Detekce podpory konkrétní feature:

```javascript
if (Office.context.requirements.isSetSupported("Settings", 1.1)) {
    // Use Office settings
} else {
    // Fall back to local storage or something else.
}
```

```javascript
if (Office.context.requirements.isSetSupported('ExcelApi', 1.1)) {
    // Do something that is only available via the new APIs
}
```

Dokumentace: [API sets](https://msdn.microsoft.com/en-us/library/office/fp142185.aspx)

Nebo specifikace features přímo v manifestu - pokud nebude splněno, aplikace se vůbec nespustí (ani neukáže v seznamu).

[Requrement Sets](https://msdn.microsoft.com/en-us/library/office/dn535871.aspx#SpecifyRequirementSets_minversion)
