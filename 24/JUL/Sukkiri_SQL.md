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