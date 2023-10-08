+++
authors = ["Muso Verda"]
title = "Разница между Observable и Subject"
date = "2023-10-08"
description = "Разберемся в разнице между Observable и Subject"
tags = [
    "rxjs",
]
categories = [
    "development"
]
+++

In stream programming there are two main interfaces: Observable and Observer.

Observable is for the consumer, it can be transformed and subscribed:

{{< highlight typescript >}}
observable.map(x => ...).filter(x => ...).subscribe(x => ...)
{{< /highlight >}}

Observer is the interface which is used to feed an observable source:

{{< highlight typescript >}}
observer.next(newItem)
{{< /highlight >}}

We can create new Observable with an Observer:

{{< highlight typescript >}}
var observable = Observable.create(observer => { 
    observer.next('first'); 
    observer.next('second'); 
    ... 
});
observable.map(x => ...).filter(x => ...).subscribe(x => ...)
{{< /highlight >}}

Or, we can use a Subject which implements both the Observable and the Observer interfaces:

{{< highlight typescript >}}
var source = new Subject();
source.map(x => ...).filter(x => ...).subscribe(x => ...)
source.next('first')
source.next('second')
{{< /highlight >}}