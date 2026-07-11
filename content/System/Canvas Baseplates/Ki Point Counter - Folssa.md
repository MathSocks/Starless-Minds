---
cssclasses:
  - kicard
  - hidetitle
  - low
kiPoints: 1
maxKiPoints: 6
dg-publish: true
---
> [!|kicard]
> ```dataviewjs
> const keys = {
>   ki: "kiPoints",
>   max: "maxKiPoints"
> }
> 
> const file = app.vault.getAbstractFileByPath(dv.current().file.path)
> 
> async function spendKi(file, amount) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     if (!(keys.ki in fm)) fm[keys.ki] = 0
>     fm[keys.ki] = Math.max(0, fm[keys.ki] - amount)
>   })
> }
> 
> async function restoreKi(file, amount) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     if (!(keys.ki in fm)) fm[keys.ki] = 0
>     if (!(keys.max in fm)) fm[keys.max] = 0
>     fm[keys.ki] = Math.min(fm[keys.ki] + amount, fm[keys.max])
>   })
> }
> 
> async function resetKi(file) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     fm[keys.ki] = fm[keys.max] ?? 0
>   })
> }
> 
> const spendBtn = dv.el('button', 'Spend Ki')
> spendBtn.onclick = () => spendKi(file, 1)
> 
> const restoreBtn = dv.el('button', 'Restore Ki')
> restoreBtn.onclick = () => restoreKi(file, 1)
> 
> const resetBtn = dv.el('button', 'Reset Ki')
> resetBtn.onclick = () => resetKi(file)
> 
> const current = dv.current()
> const ki = current[keys.ki] ?? 0
> const max = current[keys.max] ?? 0
> 
> dv.paragraph(`<h4> Ki Points: ${ki} / ${max} </h4>`)
> dv.span(spendBtn); dv.span(restoreBtn); dv.span(resetBtn)
> ```
> 