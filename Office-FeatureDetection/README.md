Office - Feature Detection
===

Detekce, zda add-in běží v aplikaci Office a selektivní načtení Office.js:

```javascript
<script>
    if (typeof Office !== "undefined") {
        var head = document.getElementsByTagName('head')[0];
        var js = document.createElement("script");

        js.type = "text/javascript";
        js.src = "https://appsforoffice.microsoft.com/lib/1/hosted/office.js";

        head.appendChild(js);
    }
</script>
```

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
