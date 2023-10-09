+++
authors = ["Muso Verda"]
title = "Observable vs Subject"
date = "2023-10-08"
description = "Разберемся в разнице между Observable и Subject."
tags = [
    "rxjs",
]
categories = [
    "development"
]
+++


### Виды Subject:

#### Subject (обыкновенный Subject)

{{< highlight typescript >}}
const subject = new Subject<number>();
subject.subscribe(v => console.log(v));

subject.next(1);    // => 1
subject.next(2);    // => 2

subject.complete();
{{< /highlight >}}

В обыкновенном Subject - подписчик реагирует на каждое событие и выводит в консоль информацию, передаваемую в событии.

#### AsyncSubject (асинхронный Subject)

{{< highlight typescript >}}
const subject = new AsyncSubject();
subject.subscribe(x => console.log(x));

subject.next(1);
subject.next(2);

subject.complete(); // => 2
{{< /highlight >}}

Асинхронный Subject уведомляет своих подписчиков только о последнем произошедшем событии и только когда Subject успешно завершается. Если асинхронный Subject завершится ошибкой, его подписчики будут уведомлены только об ошибке.

#### BehaviorSubject (поведенческий Subject)

{{< highlight typescript >}}
const subject = new BehaviorSubject(0);
subject.subscribe(x => console.log(x)); // в консоли: 0

subject.next(1); // в консоли: 1
subject.next(2); // в консоли: 2
console.log(subject.getValue()) // в консоли: 2

subject.subscribe(x => console.log(x)); // в консоли: 2
subject.complete();
{{< /highlight >}}

При подписке поведенческий Subject уведомляет своего зрителя о последнем произошедшем в нём событии или, если в Subject ещё не происходило событий, создаёт для зрителя событие с изначальной информацией. Изначальная информация передаётся при создании поведенческого Subject, в примере выше мы передаём 0 .

Важно! Поведенческий Subject имеет полезный метод .getValue() , который возвращает информацию, содержавшеюся в последнем произошедшем в Subject событии.

#### ReplaySubject (повторяющий Subject)

{{< highlight typescript >}}
const subject = new ReplaySubject();
subject.subscribe(x => console.log(x));
subject.next(1); // в консоли: 1
subject.next(2); // в консоли: 2

subject.subscribe(x => console.log(x));
// в консоли:
// 1
// 2
subject.complete();
{{< /highlight >}}

При подписке повторяющий Subject уведомляет своего зрителя о всех произошедшем в нём событиях с момента создания. Для увеличения производительности, мы можем ограничить количество событий, повторяющихся для каждого подписчика:

{{< highlight typescript >}}
const subject = new ReplaySubject(2); // будут повторяться только 2 последних события
subject.subscribe(x => console.log(x));

subject.next(1); // в консоли: 1
subject.next(2); // в консоли: 2
subject.next(3); // в консоли: 3

subject.subscribe(x => console.log(x));
// в консоли:
// 2
// 3

subject.complete();
{{< /highlight >}}

Любой вид Subject может быть преобразован в Observable с помощью метода .asObservable() .


{{< highlight typescript >}}

{{< /highlight >}}