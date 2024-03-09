[readme.mdに戻る](./readme.md)

## データベースの構築

以下のSQLを実行するとデータベースが作成されます。

```SQL
create database internet_tv;
```

## テーブルの構築

以下のSQLを実行するとテーブルが作成されます。

```SQL
use internet_tv;

create table channels (
    channel_id int primary key,
    channel_name varchar(255) not null
);

CREATE TABLE programs (
    program_id INT PRIMARY KEY,
    title VARCHAR(255) not null,
    program_detail TEXT not null,
    series_flag BOOLEAN not null
);

create table contents (
    content_id int primary key,
    program_id int,
    content_name varchar(255) not null,
    all_season_num int not null,
    all_episode_num int not null,
    foreign key (program_id) references programs(program_id)
);

create table program_frames (
    program_frame_id int primary key,
    channel_id int,
    program_id int,
    start_time datetime not null,
    end_time datetime not null,
    foreign key (channel_id) references channels (channel_id),
    foreign key (program_id) references programs(program_id)
);

create table seasons (
    season_id int primary key,
    season_number int not null
);

create table program_season (
    program_id int,
    season_id int,
    primary key (program_id, season_id),
    foreign key (program_id) references programs (program_id),
    foreign key (season_id) references seasons (season_id)
);

create table episodes (
    episode_id int primary key,
    episode_number int not null,
    episode_name varchar(255) not null,
    episode_detail text not null,
    video_time time not null,
    release_date date not null
);

create table program_episode (
    program_id int,
    episode_id int,
    primary key (program_id, episode_id),
    foreign key (program_id) references programs (program_id),
    foreign key (episode_id) references episodes (episode_id)
);

create table season_episode (
    season_id int,
    episode_id int,
    primary key (season_id, episode_id),
    foreign key (season_id) references seasons (season_id),
    foreign key (episode_id) references episodes (episode_id)
);

create table categories (
    category_id int primary key,
    category_name varchar(255) not null
);

create table program_category (
    program_id int,
    category_id int,
    primary key (program_id, category_id),
    foreign key (program_id) references programs(program_id),
    foreign key (category_id) references categories(category_id)
);

create table viewing(
    program_frame_id int,
    episode_id int,
    viewing_count int not null,
    primary key (program_frame_id, episode_id),
    foreign key (program_frame_id) references program_frames (program_frame_id),
    index idx_viewing_count (viewing_count)
);
```

## サンプルデータの挿入

以下のSQLを実行するとサンプルデータが作成されます。

```sql
insert into channels (channel_id, channel_name)
VALUES
(1, 'ドラマ'),
(2, 'ドラマ2'),
(3, 'アニメ1'),
(4, 'アニメ2'),
(5, 'スポーツ'),
(6, 'ペット'),
(7, '映画');

insert into programs (program_id,  title, program_detail, series_flag)
VALUES
(1, '鬼滅の刃 1~6話', '鬼滅の刃 1~6話の番組詳細', true),
(2, '[劇場版] 鬼滅の刃', '[劇場版] 鬼滅の刃の番組詳細', false),
(3, '鬼滅の刃 7~13話', '鬼滅の刃 7~13話の番組詳細', true),
(4, 'ドクターX シーズン2 1~6話', 'ドクターX シーズン2 1~6話の番組詳細', true),
(5, 'ドクターX シーズン2 7~13話', 'ドクターX シーズン2 7~13話の番組詳細', true),
(6, 'ドクターX シーズン3 1~6話', 'ドクターX シーズン3 1~6話の番組詳細', true);

insert into contents (content_id, program_id, content_name, all_season_num, all_episode_num)
VALUES
(1, 1, '鬼滅の刃', 2, 48),
(2, 3, '鬼滅の刃', 2, 48),
(3, 4, 'ドクターX', 6, 72),
(4, 5, 'ドクターX', 6, 72),
(5, 6, 'ドクターX', 6, 72),
(6, 6, "ドクターX", 6, 72);

insert into program_frames  (program_frame_id, channel_id, program_id, start_time, end_time)
VALUES
(1, 3, 3, concat(current_date(), ' 00:00:00'), concat(current_date(), " 03:30:00")),
(2, 4, 1, concat(current_date(), ' 03:00:00'), concat(current_date(), " 06:00:00")),
(3, 7, 2, concat(current_date(), ' 03:30:00'), concat(current_date(), " 05:00:00")),
(4, 3, 1, concat( date_sub(current_date(), interval 1 day), ' 00:00:00'), concat( date_sub(current_date(), interval 1 day), " 03:00:00")),
(5, 1, 4, concat(current_date(), ' 00:00:00'), concat(current_date(), " 06:00:00")),
(6, 2, 4, concat( date_add(current_date(), interval 1 day), ' 00:00:00'), concat( date_add(current_date(), interval 1 day), " 06:00:00")),
(7, 1, 5, concat( date_add(current_date(), interval 7 day), ' 00:00:00'), concat( date_add(current_date(), interval 7 day), " 07:00:00")),
(8, 1, 6, concat( date_add(current_date(), interval 8 day), ' 00:00:00'), concat( date_add(current_date(), interval 8 day), " 06:00:00"));

insert into viewing (program_frame_id, episode_id, viewing_count)
VALUES
(1, 1, 600),
(2, 1, 150),
(1, 2, 500),
(1, 3, 400),
(1, 4, 300),
(1, 5, 200),
(1, 6, 100),
(2, 2, 100),
(2, 3, 100),
(2, 4, 100),
(2, 5, 100),
(2, 6, 100),
(3, 38, 350),
(4, 7, 700),
(4, 8, 400),
(4, 9, 300),
(4, 10, 200),
(4, 11, 100),
(4, 12, 100),
(4, 13, 100),
(5, 39, 100),
(5, 40, 100),
(5, 41, 100),
(5, 42, 100),
(5, 43, 100),
(5, 44, 100),
(6, 39, 50),
(6, 40, 50),
(6, 41, 50),
(6, 42, 50),
(6, 43, 50),
(6, 44, 50),
(7, 45, 150),
(7, 46, 100),
(7, 47, 50),
(7, 48, 50),
(7, 49, 50),
(7, 50, 50),
(7, 51, 50),
(8, 52, 50),
(8, 53, 50),
(8, 54, 50),
(8, 55, 50),
(8, 56, 50),
(8, 57, 50);

insert into seasons (season_id, season_number)
VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6);

insert into program_season (program_id, season_id)
VALUES
(1, 1),
(3, 1),
(4, 2),
(5, 2),
(6, 3);

insert into episodes (episode_id, episode_number, episode_name, episode_detail, video_time, release_date)
VALUES
(1, 1, '鬼滅の刃1話', '鬼滅の刃1話の詳細', '00:30:00', '2021-01-01'),
(2, 2, '鬼滅の刃2話', '鬼滅の刃2話の詳細', '00:30:00', '2021-01-01'),
(3, 3, '鬼滅の刃3話', '鬼滅の刃3話の詳細', '00:30:00', '2021-01-01'),
(4, 4, '鬼滅の刃4話', '鬼滅の刃4話の詳細', '00:30:00', '2021-01-01'),
(5, 5, '鬼滅の刃5話', '鬼滅の刃5話の詳細', '00:30:00', '2021-01-01'),
(6, 6, '鬼滅の刃6話', '鬼滅の刃6話の詳細', '00:30:00', '2021-01-01'),
(7, 7, '鬼滅の刃7話', '鬼滅の刃7話の詳細', '00:30:00', '2021-01-01'),
(8, 8, '鬼滅の刃8話', '鬼滅の刃8話の詳細', '00:30:00', '2021-01-01'),
(9, 9, '鬼滅の刃9話', '鬼滅の刃9話の詳細', '00:30:00', '2021-01-01'),
(10, 10, '鬼滅の刃10話', '鬼滅の刃10話の詳細', '00:30:00', '2021-01-01'),
(11, 11, '鬼滅の刃11話', '鬼滅の刃11話の詳細', '00:30:00', '2021-01-01'),
(12, 12, '鬼滅の刃12話', '鬼滅の刃12話の詳細', '00:30:00', '2021-01-01'),
(13, 13, '鬼滅の刃13話', '鬼滅の刃13話の詳細', '00:30:00', '2021-01-01'),
(14, 14, '鬼滅の刃14話', '鬼滅の刃14話の詳細', '00:30:00', '2021-01-01'),
(15, 15, '鬼滅の刃15話', '鬼滅の刃15話の詳細', '00:30:00', '2021-01-01'),
(16, 16, '鬼滅の刃16話', '鬼滅の刃16話の詳細', '00:30:00', '2021-01-01'),
(17, 17, '鬼滅の刃17話', '鬼滅の刃17話の詳細', '00:30:00', '2021-01-01'),
(18, 18, '鬼滅の刃18話', '鬼滅の刃18話の詳細', '00:30:00', '2021-01-01'),
(19, 19, '鬼滅の刃19話', '鬼滅の刃19話の詳細', '00:30:00', '2021-01-01'),
(20, 20, '鬼滅の刃20話', '鬼滅の刃20話の詳細', '00:30:00', '2021-01-01'),
(21, 21, '鬼滅の刃21話', '鬼滅の刃21話の詳細', '00:30:00', '2021-01-01'),
(22, 22, '鬼滅の刃22話', '鬼滅の刃22話の詳細', '00:30:00', '2021-01-01'),
(23, 23, '鬼滅の刃23話', '鬼滅の刃23話の詳細', '00:30:00', '2021-01-01'),
(24, 24, '鬼滅の刃24話', '鬼滅の刃24話の詳細', '00:30:00', '2021-01-01'),
(25, 1, 'シーズン2 鬼滅の刃1話', 'シーズン2 鬼滅の刃1話の詳細', '00:30:00', '2021-01-01'),
(26, 2, 'シーズン2 鬼滅の刃2話', 'シーズン2 鬼滅の刃2話の詳細', '00:30:00', '2021-01-01'),
(27, 3, 'シーズン2 鬼滅の刃3話', 'シーズン2 鬼滅の刃3話の詳細', '00:30:00', '2021-01-01'),
(28, 4, 'シーズン2 鬼滅の刃4話', 'シーズン2 鬼滅の刃4話の詳細', '00:30:00', '2021-01-01'),
(29, 5, 'シーズン2 鬼滅の刃5話', 'シーズン2 鬼滅の刃5話の詳細', '00:30:00', '2021-01-01'),
(30, 6, 'シーズン2 鬼滅の刃6話', 'シーズン2 鬼滅の刃6話の詳細', '00:30:00', '2021-01-01'),
(31, 7, 'シーズン2 鬼滅の刃7話', 'シーズン2 鬼滅の刃7話の詳細', '00:30:00', '2021-01-01'),
(32, 8, 'シーズン2 鬼滅の刃8話', 'シーズン2 鬼滅の刃8話の詳細', '00:30:00', '2021-01-01'),
(33, 9, 'シーズン2 鬼滅の刃9話', 'シーズン2 鬼滅の刃9話の詳細', '00:30:00', '2021-01-01'),
(34, 10, 'シーズン2 鬼滅の刃10話', 'シーズン2 鬼滅の刃10話の詳細', '00:30:00', '2021-01-01'),
(35, 11, 'シーズン2 鬼滅の刃11話', 'シーズン2 鬼滅の刃11話の詳細', '00:30:00', '2021-01-01'),
(36, 12, 'シーズン2 鬼滅の刃12話', 'シーズン2 鬼滅の刃12話の詳細', '00:30:00', '2021-01-01'),
(37, 13, 'シーズン2 鬼滅の刃13話', 'シーズン2 鬼滅の刃13話の詳細', '00:30:00', '2021-01-01'),
(38, 1, '[劇場版] 鬼滅の刃', '[劇場版] 鬼滅の刃の詳細', '01:30:00', '2021-01-01'),
(39, 1, 'ドクターX シーズン2 1話', 'ドクターX シーズン2 1話の詳細', '01:00:00', '2021-01-01'),
(40, 2, 'ドクターX シーズン2 2話', 'ドクターX シーズン2 2話の詳細', '01:00:00', '2021-01-01'),
(41, 3, 'ドクターX シーズン2 3話', 'ドクターX シーズン2 3話の詳細', '01:00:00', '2021-01-01'),
(42, 4, 'ドクターX シーズン2 4話', 'ドクターX シーズン2 4話の詳細', '01:00:00', '2021-01-01'),
(43, 5, 'ドクターX シーズン2 5話', 'ドクターX シーズン2 5話の詳細', '01:00:00', '2021-01-01'),
(44, 6, 'ドクターX シーズン2 6話', 'ドクターX シーズン2 6話の詳細', '01:00:00', '2021-01-01'),
(45, 7, 'ドクターX シーズン2 7話', 'ドクターX シーズン2 7話の詳細', '01:00:00', '2021-01-01'),
(46, 8, 'ドクターX シーズン2 8話', 'ドクターX シーズン2 8話の詳細', '01:00:00', '2021-01-01'),
(47, 9, 'ドクターX シーズン2 9話', 'ドクターX シーズン2 9話の詳細', '01:00:00', '2021-01-01'),
(48, 10, 'ドクターX シーズン2 10話', 'ドクターX シーズン2 10話の詳細', '01:00:00', '2021-01-01'),
(49, 11, 'ドクターX シーズン2 11話', 'ドクターX シーズン2 11話の詳細', '01:00:00', '2021-01-01'),
(50, 12, 'ドクターX シーズン2 12話', 'ドクターX シーズン2 12話の詳細', '01:00:00', '2021-01-01'),
(51, 13, 'ドクターX シーズン2 13話', 'ドクターX シーズン2 13話の詳細', '01:00:00', '2021-01-01'),
(52, 1, 'ドクターX シーズン3 1話', 'ドクターX シーズン3 1話の詳細', '01:00:00', '2021-01-01'),
(53, 2, 'ドクターX シーズン3 2話', 'ドクターX シーズン3 2話の詳細', '01:00:00', '2021-01-01'),
(54, 3, 'ドクターX シーズン3 3話', 'ドクターX シーズン3 3話の詳細', '01:00:00', '2021-01-01'),
(55, 4, 'ドクターX シーズン3 4話', 'ドクターX シーズン3 4話の詳細', '01:00:00', '2021-01-01'),
(56, 5, 'ドクターX シーズン3 5話', 'ドクターX シーズン3 5話の詳細', '01:00:00', '2021-01-01'),
(57, 6, 'ドクターX シーズン3 6話', 'ドクターX シーズン3 6話の詳細', '01:00:00', '2021-01-01');

insert into program_episode (program_id, episode_id)
VALUES
(1, 1),
(1, 2),
(1, 3),
(1, 4),
(1, 5),
(1, 6),
(2, 38),
(3, 7),
(3, 8),
(3, 9),
(3, 10),
(3, 11),
(3, 12),
(3, 13),
(4, 39),
(4, 40),
(4, 41),
(4, 42),
(4, 43),
(4, 44),
(5, 39),
(5, 40),
(5, 41),
(5, 42),
(5, 43),
(5, 44),
(6, 52),
(6, 53),
(6, 54),
(6, 55),
(6, 56),
(6, 57);

insert into season_episode (season_id, episode_id)
VALUES
(1, 1),
(1, 2),
(1, 3),
(1, 4),
(1, 5),
(1, 6),
(1, 7),
(1, 8),
(1, 9),
(1, 10),
(1, 11),
(1, 12),
(1, 13),
(1, 14),
(1, 15),
(1, 16),
(1, 17),
(1, 18),
(1, 19),
(1, 20),
(1, 21),
(1, 22),
(1, 23),
(1, 24),
(2, 25),
(2, 26),
(2, 27),
(2, 28),
(2, 29),
(2, 30),
(2, 31),
(2, 32),
(2, 33),
(2, 34),
(2, 35),
(2, 36),
(2, 37),
(2, 39),
(2, 40),
(2, 41),
(2, 42),
(2, 43),
(2, 44),
(2, 45),
(2, 46),
(2, 47),
(2, 48),
(2, 49),
(2, 50),
(2, 51),
(3, 52),
(3, 53),
(3, 54),
(3, 55),
(3, 56),
(3, 57);


insert into categories (category_id, category_name)
VALUES
(1, 'アニメ'),
(2, '映画'),
(3, 'ドラマ'),
(4, 'ニュース');


insert into program_category (program_id, category_id)
VALUES
(1,1),
(2,1),
(2,2),
(3,1),
(4,3),
(5,3),
(6,3);
```
[readme.mdに戻る](./readme.md)
