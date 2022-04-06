# Lifecycle

### Event sequence (в порядке приоритета)

```
ngOnChanges() — когда изменяются или сбрасываются входные параметры
ngOnInit() — для инициализации директивы или компонента. Вызывается один раз после инициализации параметров (после первого ngOnChanges())
ngDoCheck() — 
ngAfterContentInit() — вызывается один раз после ngDoCheck()
ngAfterContentChecked() — после ngAfterContentInit() и каждого ngDoCheck()
ngAfterViewInit() – один раз после первого ngAfterContentChecked()
ngAfterViewChecked() — после ngAfterViewInit() и каждого ngAfterContentChecked()
ngOnDestroy() — вызывается непосредственно перед уничтожением директивы или компонента

```
