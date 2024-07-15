### スッキリSQL
- 3章 練習問題
1. SELECT * FROM 気象観測 WHERE 月 = 6;
2. SELECT * FROM 気象観測 WHERE NOT 月 = 6; -> <> 6; 6月以外(6に等しくない)
3. SELECT * FROM 気象観測 WHERE 降水量 <= 100; -> < 100; 100未満
4. SELECT * FROM 気象観測 WHERE 降水量 > 200;
5. SELECT * FROM 気象観測 WHERE 最高気温 > 30; –> >= 30; 30以上
6. SELECT * FROM 気象観測 WHERE 最低気温 < 0; -> <= 0; 0以下
7. SELECT * FROM 気象観測 WHERE 月 3 AND 月 5 AND 月 7; -> WHERE 月 IN (3, 5, 7); 月指定
   SELECT * FROM 気象観測 WHERE 月 = 3 OR 月 = 5 OR 月 = 7;
8. SELECT * FROM 気象観測 WHERE 月 NOT IN (3, 7, 5);
   SELECT * FROM 気象観測 WHERE 月 <> 3 OR 月 <> 5 OR 月<>7;
9. SELECT * FROM 気象観測 WHERE 降水量 < 100 AND 湿度 < <= 50; -> <= 100 AND 湿度 < <= 50;
10. SELECT * FROM 気象観測 WHERE 最低気温 < 5 OR 最高気温 > 35;
11. SELECT * FROM 気象観測 WHERE 湿度 BETWEEN 60 AND 79;
    SELECT * FROM 気象観測 WHERE 湿度 >= 60 AND 湿度 <= 79;
12. SELECT * FROM 気象観測 WHERE 月 IS NULL; -> WHERE 降水量 IS NULL OR 最高気温 IS NULL OR 最低気温 IS NULL OR 湿度 IS NULL
    (観測データのない列、がある月)

1. SELECT 都道府県名 FROM 都道府県 WHERE 都道府県名 LIKE '%川';
2. SELECT 都道府県名 FROM 都道府県 WHERE 都道府県名 LIKE '%島%;
3. SELECT 都道府県名 FROM 都道府県 WHERE 都道府県名 LIKE '愛%';
5. SELECT 都道府県名 FROM 都道府県 WHERE 都道府県名 = 県庁所在地;
4. SELECT 都道府県名 FROM 都道府県 WHERE 都道府県名 <> 県庁所在地;

1. SELECT * FROM 成績表;
2. INSERT INTO 成績表 VALUE ('S001', '織田 信長', 77, 55, 80, 75, 93, NULL);
3. UPDATE 成績表 SET 法学 = 85, 哲学 = 67 WHERE 学籍番号 = 'S001';
4. UPDATE 成績表 SET 外国語 = 81 WHERE 学籍番号 = 'A002' AND 学籍番号 = 'E003';
5. 1) UPDATE 成績表 SET 総合成績 = 'A' WHERE 法学 >= 80 AND 経済学 >= 80 AND 哲学 >= 80 AND 情報理論 >= 80 AND 外国語 >= 80;
   2) UPDATE 成績表 SET 総合成績 = 'B' WHERE (法学 >= 80 OR 外国語 >= 80) AND (経済学 >= 80 OR 哲学 >= 80) AND 総合成績 IS NULL;
   3) UPDATE 成績表 SET 総合成績 = 'D' WHERE  法学 < 50 AND 経済学 < 50 AND 哲学 < 50 AND 情報理論 < 50 AND 外国語 < 50 AND 総合成績 IS NULL;
   4) UPDATE 成績表 SET 総合成績 = 'C' WHERE 総合成績 IS NULL;
6. DELETE FROM 成績表 WHERE 法学 = 0 OR 経済学 = 0 OR 哲学 = 0 OR 情報理論 = 0 OR 外国語 = 0;

- 4章 練習問題
1. SELECT * FROM 注文履歴 ORDER BY 注文番号, 注文枝番;
2. SELECT DISTINCT 商品名 FROM 注文履歴 WHERE 日付 >= '2018-01-01' AND 日付 <= '2018-01-31' ORDER BY 商品名;
3. SELECT 注文番号, 注文枝番, 注文金額 FROM 注文履歴 WHERE 分類 = '1' ORDER BY 注文金額 OFFSET 1 ROWS FETCH NEXT 3 ROWS ONLY
4. SELECT 日付, 商品名, 単価, 数量, 注文金額 FROM 注文履歴 WHERE 分類 = '3' AND 数量 >= 2 ORDER BY 日付、数量 DESC;
   (3: その他の商品について、2つ以上同時に購入された商品を取得し、日付, 商品名, 単価, 数量, 注文金額 同日に売り上げたものは数量の多い順)
5. SELECT DISTINCT 分類, 商品名, NULL, 単価 FROM 注文履歴 WHERE 分類 = '2' UNION
   SELECT DISTINCT 分類, 商品名, NULL, 単価 FROM 注文履歴 WHERE 分類 = '3' ORDER BY 1, 2

- 5章 練習問題
1. UPDATE 試験結果 SET 午後1 = (80*4) - (86+68+91) WHERE 受験者ID = 'SW1064'
   UPDATE 試験結果 SET 論述 = (68*4) - (65+53+70) WHERE 受験者ID = 'SW1350'
   UPDATE 試験結果 SET 午前 = (56*4) = (59+56+36) WHERE 受験者ID = 'SW1877'
2. SELECT 受験者ID AS '合格者ID' FROM 試験結果 WHERE 午前 >= 60 AND (午後1 + 午後2) >= 120 AND 論述 >= (午前+午後1+午後2+論述)*0.3

1. UPDATE 回答者 SET 国名 CASE SUBSTRING (TRIM(メールアドレス), LENGTH (TRIM(メールアドレス))-1 ,2)
   WHEN 'jp' THEN '日本'
   WHEN 'uk' THEN 'イギリス'
   WHEN 'cn' THEN '中国'
   WHEN 'fr' THEN 'フランス'
   WHEN 'vn' THEN 'ベトナム'　END
2. SELECT TRIM(メールアドレス) AS メールアドレス,
   CASE WHEN 年齢 >= 20 AND 年齢 < 30 THEN '20代'
   　　　WHEN 年齢 >= 30 AND 年齢 < 40 THEN '30代'
        WHEN 年齢 >= 40 AND 年齢 < 50 THEN '40代'
        WHEN 年齢 >= 50 AND 年齢 < 60 THEN '50代' END
        || ':' ||
    CASE 性別 WHEN 'M' THEN '男性'
             WHEN 'F' THEN '女性' END AS 属性
    FROM 回答者