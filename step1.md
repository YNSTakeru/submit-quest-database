[readme.mdに戻る](./readme.md)

## テーブル作成

テーブル: categories
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| category_id | int(11) |  | PRIMARY |  | YES |
| category_name | varchar(255) |  |  |  |  |

テーブル: programs
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| program_id | int(11) |  | PRIMARY |  | YES |
| title | varchar(255) |  |  |  |  |
| program_detail | text |  |  |  |  |
| series_flag | tinyint(1) |  |  |  |  |

テーブル: seasons
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| season_id | int(11) |  | PRIMARY |  | YES |
| season_number |int(11) |  |  |  |  |

テーブル: program_season
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| program_id | int(11) |  | FOREIGN |  | |
| season_id |int(11) |  | FOREIGN  |  |  |

テーブル: episodes
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| episode_id | int(11) |  | PRIMARY |  | YES |
| episode_number |int(11) |  |  |  |  |
| episode_name |varchar(255) |  |  |  |  |
| episode_detail | text |  |  |  |  |
| video_time | time |  |  |  |  |
| release_date | date |  |  |  |  |

テーブル: season_episode
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| season_id | int(11) |  | FOREIGN |  | |
| episode_id |int(11) |  | FOREIGN  |  |  |


テーブル: contents
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| content_id | int(11) |  | PRIMARY |  | YES |
| program_id | int(11) |  |FOREIGN  |  |  |
| content_name | varchar(255) |  |  |  |  |
| all_season_num | int(11) |  |  |  |  |
| all_episode_num | int(11) |  |  |  |  |

テーブル: program_category
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| program_id | int(11) |  | FOREIGN |  |  |
| category_id | int(11) |  | FOREIGN |  |  |

テーブル: channels
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| channel_id | int(11) |  | PRIMARY |  | YES |
| channel_name | varchar(255) |  |  |  |  |

テーブル: program_frames
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| program_frame_id | int(11) |  | PRIMARY |  | YES |
| channel_id | int(11) |  | FOREIGN |  |  |
| program_id | int(11) |  | FOREIGN |  |  |
| start_time | datetime |  | |  |  |
| end_time | datetime |  | |  |  |

テーブル: viewing
| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| ---- | ---- | --- | --- | --- | --- |
| program_frame_id | int(11) |  | FOREIGN |  |  |
| episode_id | int(11) |  | FOREIGN |  |  |
| viewing_count | int(11) |  | INDEX |  |  |

[readme.mdに戻る](./readme.md)
