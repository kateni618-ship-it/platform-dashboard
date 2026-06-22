# **附件：DM层数据表需求**

## 表一：达人用户信息表

```
dim_creator_info
```

记录达人基础属性、等级资质、内容偏好与注册审核信息，作为其他分析表的用户维度基础维表。

| 字段名                    | 字段说明             | 数据类型  | 备注                        |
| ------------------------- | -------------------- | --------- | --------------------------- |
| `creator_id`              | 达人唯一ID           | STRING    | 主键                        |
| `username`                | 平台用户名           | STRING    | —                           |
| `email`                   | 注册邮箱             | STRING    | 脱敏处理                    |
| `phone`                   | 手机号               | STRING    | 脱敏处理                    |
| `country`                 | 所在国家             | STRING    | 注册填写                    |
| `state`                   | 所在州               | STRING    | 注册填写                    |
| `register_time`           | 注册时间             | TIMESTAMP | 系统记录                    |
| `register_channel`        | 注册来源渠道         | STRING    | 如：google/referral/organic |
| `account_status`          | 账号状态             | STRING    | active/suspended/cancelled  |
| `apply_time`              | 达人申请提交时间     | TIMESTAMP | 系统记录                    |
| `approve_time`            | 达人审核通过时间     | TIMESTAMP | 系统记录                    |
| `approve_status`          | 审核状态             | STRING    | pending/approved/rejected   |
| `reject_reason`           | 审核拒绝原因         | STRING    | 审核拒绝时填写              |
| `creator_level`           | 达人等级             | STRING    | 如：L1/L2/L3/L4             |
| `level_update_time`       | 等级最近更新时间     | TIMESTAMP | 系统计算更新                |
| `level_upgrade_condition` | 当前等级晋升条件描述 | STRING    | 如：GMV/订单数门槛          |
| `preferred_category_l1`   | 偏好一级类目         | STRING    | 用户设置/行为计算           |
| `preferred_category_l2`   | 偏好二级类目         | STRING    | 用户设置/行为计算           |
| `preferred_content_style` | 偏好内容风格标签     | ARRAY     | 如：beauty/lifestyle/tech   |
| `preferred_platform`      | 偏好发布平台         | ARRAY     | 如：tiktok/instagram        |
| `tiktok_handle`           | TikTok账号           | STRING    | 用户绑定                    |
| `tiktok_followers`        | TikTok粉丝数         | BIGINT    | API同步                     |
| `instagram_handle`        | Instagram账号        | STRING    | 用户绑定                    |
| `instagram_followers`     | Instagram粉丝数      | BIGINT    | API同步                     |
| `youtube_handle`          | YouTube账号          | STRING    | 用户绑定                    |
| `youtube_subscribers`     | YouTube订阅数        | BIGINT    | API同步                     |
| `last_login_time`         | 最近登录时间         | TIMESTAMP | 系统记录                    |
| `last_active_time`        | 最近活跃时间         | TIMESTAMP | 系统记录                    |

------

## 表二：产品模块流量及行为统计表

```
fact_module_traffic_behavior
```

以「模块 × 日期」为统计粒度，记录各产品模块及分层的流量与用户选品行为数据，支撑模块效果评估与选品转化分析。

| 字段名                 | 字段说明          | 数据类型 | 备注                                                         |
| ---------------------- | ----------------- | -------- | ------------------------------------------------------------ |
| **维度字段**           |                   |          |                                                              |
| `stat_date`            | 统计日期          | DATE     | 分区字段                                                     |
| `page_type`            | 所属页面          | STRING   | home/商品集/JFU/内容模块/商品feeds流/我的Pick/我的创作/我的成就/商品详情页 |
| `module_name`          | 模块名称          | STRING   | 如：Bestseller/Trending/New Arrival/Creator Pick/本地仓/Just for U |
| `module_level`         | 模块分层级别      | STRING   | L1/L2/L3                                                     |
| `module_level_name`    | 模块分层名称      | STRING   | 对应各层级具体名称                                           |
| **流量指标**           |                   |          |                                                              |
| `expose_pv`            | 曝光量（IPV）     | BIGINT   | 模块总曝光次数                                               |
| `click_pv`             | 点击量（IPV）     | BIGINT   | 模块总点击次数                                               |
| `expose_uv`            | 曝光用户数（UV）  | BIGINT   | 模块曝光独立用户数                                           |
| `click_uv`             | 点击用户数（UV）  | BIGINT   | 模块点击独立用户数                                           |
| `ctr`                  | 模块点击率        | FLOAT    | click_uv / expose_uv                                         |
| `avg_stay_duration`    | 平均停留时长（s） | FLOAT    | 用户在该模块平均停留秒数                                     |
| **商品曝光行为**       |                   |          |                                                              |
| `product_expose_count` | 曝光商品数        | BIGINT   | 模块内被曝光的商品总数                                       |
| `product_click_count`  | 点击商品数        | BIGINT   | 模块内被点击的商品总数                                       |
| `product_expose_ipv`   | 商品曝光IPV       | BIGINT   | 模块内商品曝光总次数                                         |
| `product_click_ipv`    | 商品详情点击IPV   | BIGINT   | 模块内商品详情点击总次数                                     |
| `product_expose_uv`    | 商品曝光UV        | BIGINT   | 有商品曝光行为的独立用户数                                   |
| `product_click_uv`     | 商品详情点击UV    | BIGINT   | 有商品详情点击行为的独立用户数                               |
| **选品行为**           |                   |          |                                                              |
| `pick_product_count`   | Pick商品数        | BIGINT   | 模块内被Pick的商品总数                                       |
| `pick_ipv`             | 商品Pick IPV      | BIGINT   | 模块内Pick行为总次数                                         |
| `pick_uv`              | 商品Pick UV       | BIGINT   | 有Pick行为的独立用户数                                       |
| `pick_rate`            | Pick转化率        | FLOAT    | pick_uv / product_click_uv                                   |
| **创作行为**           |                   |          |                                                              |
| `create_product_count` | Create商品数      | BIGINT   | 模块内触发创作的商品总数                                     |
| `create_ipv`           | 商品Create IPV    | BIGINT   | 模块内Create行为总次数                                       |
| `create_uv`            | 商品Create UV     | BIGINT   | 有Create行为的独立用户数                                     |
| `create_rate`          | Create转化率      | FLOAT    | create_uv / pick_uv                                          |

------

## 表三：用户行为明细表

```
dwd_creator_behavior_detail
```

以用户单次行为事件为粒度的明细记录表，保留完整行为上下文，支撑路径分析、漏斗归因与个性化推荐迭代。

| 字段名              | 字段说明           | 数据类型  | 备注                          |
| ------------------- | ------------------ | --------- | ----------------------------- |
| `event_id`          | 事件唯一ID         | STRING    | 主键，UUID                    |
| `creator_id`        | 达人用户ID         | STRING    | 关联用户信息表                |
| `session_id`        | 会话ID             | STRING    | 同一次登录会话内唯一          |
| `event_time`        | 事件发生时间       | TIMESTAMP | 精确到毫秒                    |
| `stat_date`         | 事件日期           | DATE      | 分区字段                      |
| `expose_module`     | 曝光模块           | STRING    | 商品集/JFU/内容模块/feeds流等 |
| `click_module`      | 点击模块           | STRING    | 实际发生点击的模块            |
| `module_level`      | 模块分层           | STRING    | L1/L2/L3                      |
| `module_level_name` | 模块分层名称       | STRING    | 对应层级具体名称              |
| `product_id`        | 商品ID（MUSE SPU） | STRING    | 平台商品唯一标识              |
| `is_picked`         | 是否Pick           | BOOLEAN   | true/false                    |
| `is_created`        | 是否Create         | BOOLEAN   | true/false                    |
| `pick_time`         | Pick发生时间       | TIMESTAMP | 未Pick则为空                  |
| `create_time`       | Create发生时间     | TIMESTAMP | 未Create则为空                |
| `device_type`       | 设备类型           | STRING    | web/ios/android               |
| `os_version`        | 操作系统版本       | STRING    | —                             |
| `app_version`       | App版本号          | STRING    | —                             |
| `referrer_page`     | 来源页面           | STRING    | 上一个页面标识                |

------

## 表四：商品行为统计表

```
fact_product_behavior_stats
```

以「商品 × 日期」为统计粒度，汇总商品在全平台各环节的行为表现，支撑选品效果评估、商品质量分级与推荐策略优化。

| 字段名                     | 字段说明                  | 数据类型 | 备注                         |
| -------------------------- | ------------------------- | -------- | ---------------------------- |
| **维度字段**               |                           |          |                              |
| `stat_date`                | 统计日期                  | DATE     | 分区字段                     |
| `product_id`               | 商品ID（MUSE SPU）        | STRING   | 主键                         |
| `product_title`            | 商品标题                  | STRING   | —                            |
| `product_price`            | 商品价格                  |          |                              |
| `product_profilt`          | 商品佣金                  |          |                              |
| `product_url`              | 商品落地页URL             | STRING   | —                            |
| `product_image_url`        | 商品首图URL               | STRING   | —                            |
| `category_l1`              | 一级类目                  | STRING   | 如：Beauty                   |
| `category_l2`              | 二级类目                  | STRING   | 如：Skincare                 |
| `category_l3`              | 三级类目                  | STRING   | 如：Face Serum               |
| **曝光行为**               |                           |          |                              |
| `expose_ipv`               | 曝光IPV                   | BIGINT   | 商品被曝光总次数             |
| `expose_uv`                | 曝光UV                    | BIGINT   | 商品曝光独立用户数           |
| **详情点击行为**           |                           |          |                              |
| `click_ipv`                | 点击IPV                   | BIGINT   | 商品详情页点击总次数         |
| `click_uv`                 | 点击UV                    | BIGINT   | 商品详情页点击独立用户数     |
| `ctr`                      | 点击率                    | FLOAT    | click_uv / expose_uv         |
| **Pick行为**               |                           |          |                              |
| `pick_ipv`                 | Pick IPV                  | BIGINT   | 商品被Pick总次数             |
| `pick_uv`                  | Pick UV                   | BIGINT   | Pick该商品的独立用户数       |
| `pick_rate`                | Pick率                    | FLOAT    | pick_uv / click_uv           |
| **Create行为**             |                           |          |                              |
| `create_ipv`               | Create IPV                | BIGINT   | 商品触发Create总次数         |
| `create_uv`                | Create UV                 | BIGINT   | 触发Create的独立用户数       |
| `create_rate`              | Create率                  | FLOAT    | create_uv / pick_uv          |
| **深度行为**               |                           |          |                              |
| `avg_detail_stay_duration` | 详情页平均停留时长（s）   | FLOAT    | 用户在商品详情页平均停留     |
| `avg_create_stay_duration` | Create页平均停留时长（s） | FLOAT    | 用户在创作页平均停留         |
| `create_works_count`       | Create作品数              | BIGINT   | 基于该商品产出的创作内容总数 |
| `is_published`             | 是否有发布行为            | BOOLEAN  | 该商品是否有内容被发布上线   |
| `publish_count`            | 发布内容总数              | BIGINT   | 基于该商品发布的内容总数     |

------

## 表间关系说明

```
dim_creator_info          维度表（用户基础信息）
        ↓ creator_id
dwd_creator_behavior_detail   明细事实表（行为明细）
        ↓ 聚合
fact_module_traffic_behavior  统计事实表（模块维度汇总）
        ↓ product_id
fact_product_behavior_stats   统计事实表（商品维度汇总）
```