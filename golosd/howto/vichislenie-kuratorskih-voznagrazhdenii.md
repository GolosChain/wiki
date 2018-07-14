# Как вычислять реальные кураторские до закрытия окна выплат

Автор [@**vvk**](https://golos.io/@vvk)

Рассмотрим, как прогнозировать кураторские вознаграждения до закрытия поста.

# Общие сведения

В механику заложена отдача 25% от полной выплаты поста кураторам, однако кураторы, голосующие в первые 30 минут, отдают часть своей кураторской доли автору \(штрафуются\) по линейной зависимости. 30 минут - это 100%, соответственно голосующий на 15-й минуте отдаёт 50% своего кураторского вознаграждения.

# Как получить реальный кураторский процент

У нас есть следующие параметры:

* `weight`
  - вес куратора, находится в
  `active_votes`
  , с учётом штрафа
* `total_vote_weight`
  - сумма всех весов кураторов без учёта штрафа

Таким образом, мы можем вычислить реальный кураторский процент так:

```
sum_weight = int()

for vote in active_votes:
    sum_weight += int(vote['weight'])

curators_share = sum_weight / int(post['total_vote_weight']) * 0.25

```

# Механика

Дальше рассмотрим, а как вообще вычисляется weight каждого куратора.

В модели у нас будет:

* 5 апов с одинаковыми условными 50 rshares \(
  `votes`
  \).
* `s`
  - это CONTENT\_CONSTANT, в модели она маленькая для наглядности
* 10000 - это
  `std::numeric_limits`
  `<`
  `uint64_t`
  `>`
  `::max()`
  , опять же маленькое значение для наглядности

```
# modeling weight

votes = [50, 50, 50, 50, 50]
#votes = [10, 50, 100, 150, 100, 150, 50, 100000, 200]

s = 100
num_votes = 10

sum_vote_rshares = int()

for rshares in votes:
    old_weight = (10000 * sum_vote_rshares) / (2 * s + sum_vote_rshares)
    sum_vote_rshares += rshares
    new_weight = (10000 * sum_vote_rshares) / (2 * s + sum_vote_rshares)
    weight = new_weight - old_weight
    print('vote rshares: {:.0f}: old: {:.0f}, new: {:.0f}, weight: {:.0f}'.format(rshares, old_weight, new_weight, weight))

```

Выполнение этого кода даст такую картину:

```
vote rshares: 50: old: 0, new: 2000, weight: 2000
vote rshares: 50: old: 2000, new: 3333, weight: 1333
vote rshares: 50: old: 3333, new: 4286, weight: 952
vote rshares: 50: old: 4286, new: 5000, weight: 714
vote rshares: 50: old: 5000, new: 5556, weight: 556

```

Видим, что хотя все апы одинаковы, вес каждого последующего меньше предыдущего. Вот так и работают вычисления в golosd, однако там ещё после вычисления`weight`накладывается штраф, если ап сделан в первые 30 минут. Т.е. например для апа на 15-й минуте его вес уменьшается так:`weight = weight * 0.5`

Зная данную механику, теперь можно вычислять кураторские ещё до закрытия поста:

```

active_votes = sorted(post['active_votes'], key=lambda k: datetime.strptime(k['time'], '%Y-%m-%dT%H:%M:%S'))

for vote in active_votes:
    reward = pending_payout * 0.25 * investor['weight'] / int(post['total_vote_weight'])
    time_passed = datetime.strptime(vote['time'], '%Y-%m-%dT%H:%M:%S') - post['created']
    print('real weight percent: {:.2%}, reward: {:.3f} GBG, voter: {}, time: {}, weight: {:.0f}'.format(
        real_share, reward, vote['voter'], time_passed, float(vote['weight'])/100000000000000))

```

Пример вывода:

```
real weight percent: 0.00%, reward: 0.000 GBG, voter: zaebot, time: 0:00:03, weight: 0
real weight percent: 0.00%, reward: 0.000 GBG, voter: rastabandito, time: 0:00:03, weight: 0
real weight percent: 0.00%, reward: 0.265 GBG, voter: nett, time: 0:03:03, weight: 336
real weight percent: 0.00%, reward: 2.148 GBG, voter: idestus, time: 0:10:03, weight: 2721
real weight percent: 0.00%, reward: 4.378 GBG, voter: cyros, time: 0:10:36, weight: 5546
real weight percent: 0.00%, reward: 0.017 GBG, voter: marusya, time: 0:10:39, weight: 21
real weight percent: 0.00%, reward: 2.120 GBG, voter: sveokla, time: 0:20:03, weight: 2685
real weight percent: 0.00%, reward: 0.960 GBG, voter: graff0x, time: 0:21:03, weight: 1216
real weight percent: 0.00%, reward: 0.162 GBG, voter: grkh, time: 0:23:03, weight: 205
real weight percent: 0.00%, reward: 0.004 GBG, voter: usias60, time: 0:25:03, weight: 4
real weight percent: 0.00%, reward: 35.749 GBG, voter: spinner, time: 0:26:21, weight: 45282
real weight percent: 0.00%, reward: 21.262 GBG, voter: park.bom, time: 0:27:03, weight: 26932
real weight percent: 0.00%, reward: 1.664 GBG, voter: svamiva, time: 0:27:33, weight: 2108
real weight percent: 0.00%, reward: 0.173 GBG, voter: yakubovruslan, time: 0:29:03, weight: 219

```

Здесь у нас GBG, хотя кураторские идут в СГ, кому надо легко вычислят.



[Источник](https://golos.io/goldvoice/@vvk/curation-rewards)

