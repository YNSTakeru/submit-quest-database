[readme.mdに戻る](./readme.md)
## 1 よく見られているエピソードを知りたいです。エピソード視聴数トップ3のエピソードタイトルと視聴数を取得してください

```sql
select e.episode_name "エピソードタイトル", sum(viewing_count) "視聴数"
from viewing as v
join episodes as e
on v.episode_id = e.episode_id
group by v.episode_id
order by sum(viewing_count) desc
limit 3;
```

## 2 よく見られているエピソードの番組情報やシーズン情報も合わせて知りたいです。エピソード視聴数トップ3の番組タイトル、シーズン数、エピソード数、エピソードタイトル、視聴数を取得してください

```sql
select p.title "番組タイトル", ifnull(c.all_season_num,"単発作品です")  "総シーズン数", ifnull(s.season_number, "単発作品です") "シーズン番号",
ifnull(c.all_episode_num,"単発作品です") "総エピソード数", top3_viewing.episode_number "エピソード番号", top3_viewing.episode_name "エピソードタイトル", top3_viewing.viewing_count "視聴数"
from (
    select e.episode_id, e.episode_name, e.episode_number, sum(viewing_count) as viewing_count
    from viewing as v
    join episodes as e
    on v.episode_id = e.episode_id
    group by v.episode_id
    order by viewing_count desc
    limit 3
) as top3_viewing
join program_episode as pe
on top3_viewing.episode_id = pe.episode_id
join programs as p
on pe.program_id = p.program_id
left join season_episode as se
on top3_viewing.episode_id = se.episode_id
left join seasons as s
on se.season_id = s.season_id
left join contents as c
on p.program_id = c.program_id
order by top3_viewing.viewing_count desc;
```

## 3 本日の番組表を表示するために、本日、どのチャンネルの、何時から、何の番組が放送されるのかを知りたいです。本日放送される全ての番組に対して、チャンネル名、放送開始時刻(日付+時間)、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を取得してください。なお、番組の開始時刻が本日のものを本日方法される番組とみなすものとします

```sql
select c.channel_name "チャンネル名", pf.start_time "放送開始時刻(日付+時間)", pf.end_time "放送終了時刻", ifnull(co.all_season_num,"単発作品です")  "総シーズン数", ifnull(s.season_number, "単発作品です") "シーズン番号",
ifnull(co.all_episode_num,"単発作品です") "総エピソード数", e.episode_number "エピソード番号", e.episode_name "エピソードタイトル", e.episode_detail "エピソード詳細"
from program_frames as pf
join channels as c
on pf.channel_id = c.channel_id
join programs as p
on pf.program_id = p.program_id
left join program_episode as pe
on p.program_id = pe.program_id
left join episodes as e
on pe.episode_id = e.episode_id
left join season_episode as se
on e.episode_id = se.episode_id
left join seasons as s
on se.season_id = s.season_id
left join contents as co
on p.program_id = co.program_id
where date(pf.start_time) = current_date();
```

## 4 ドラマというチャンネルがあったとして、ドラマのチャンネルの番組表を表示するために、本日から一週間分、何日の何時から何の番組が放送されるのかを知りたいです。ドラマのチャンネルに対して、放送開始時刻、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を本日から一週間分取得してください

```sql
select pf.start_time "放送開始時刻", pf.end_time "放送終了時刻", ifnull(co.all_season_num,"単発作品です")  "総シーズン数", ifnull(s.season_number, "単発作品です") "シーズン番号",
ifnull(co.all_episode_num,"単発作品です") "総エピソード数", e.episode_number "エピソード番号", e.episode_name "エピソードタイトル", e.episode_detail "エピソード詳細"
from program_frames as pf
join channels as c
on pf.channel_id = c.channel_id
join programs as p
on pf.program_id = p.program_id
left join program_episode as pe
on p.program_id = pe.program_id
left join episodes as e
on pe.episode_id = e.episode_id
left join season_episode as se
on e.episode_id = se.episode_id
left join seasons as s
on se.season_id = s.season_id
left join contents as co
on p.program_id = co.program_id
where date(pf.start_time) between current_date() and date_add(current_date(), interval 7 day) and c.channel_name = 'ドラマ';
```

## 5 (advanced) 直近一週間で最も見られた番組が知りたいです。直近一週間に放送された番組の中で、エピソード視聴数合計トップ2の番組に対して、番組タイトル、視聴数を取得してください

```sql
select p.title "番組タイトル", sum(viewing_count) "視聴数"
from viewing as v
join program_frames as pf
on v.program_frame_id = pf.program_frame_id
join programs as p
on pf.program_id = p.program_id
where date(pf.start_time) between date_sub(current_date(), interval 7 day) and current_date()
group by pf.program_id
order by sum(viewing_count) desc
limit 2;
```

## 6 (advanced) ジャンルごとの番組の視聴数ランキングを知りたいです。番組の視聴数ランキングはエピソードの平均視聴数ランキングとします。ジャンルごとに視聴数トップの番組に対して、ジャンル名、番組タイトル、エピソード平均視聴数を取得してください。

```sql
select ca.category_name "ジャンル名", p.title "番組タイトル", t1.max_avg_viewing_count "エピソード平均視聴数"
from (
	select pc.category_id, max(pidvavg.avg_viewing_count) as max_avg_viewing_count
	from (
		select p.program_id, p.title, avg(v.viewing_count) as avg_viewing_count from viewing v
		join program_frames pf
		on v.program_frame_id = pf.program_frame_id
		join programs p
		on pf.program_id = p.program_id
		group by p.program_id
		order by avg(v.viewing_count) desc
	) as pidvavg
	join program_category pc
	on pidvavg.program_id = pc.program_id
	group by pc.category_id
) as t1
left join (
	select pidvavg.program_id, pc.category_id, pidvavg.avg_viewing_count
	from (
		select p.program_id, p.title, avg(v.viewing_count) as avg_viewing_count from viewing v
		join program_frames pf
		on v.program_frame_id = pf.program_frame_id
		join programs p
		on pf.program_id = p.program_id
		group by p.program_id
		order by avg(v.viewing_count) desc
	) as pidvavg
	join program_category pc
	on pidvavg.program_id = pc.program_id
) as t2
on t1.category_id = t2.category_id and t1.max_avg_viewing_count = t2.avg_viewing_count
join programs p
on t2.program_id = p.program_id
join categories ca
on ca.category_id = t2.category_id;
```
[readme.mdに戻る](./readme.md)
