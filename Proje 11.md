test veritabanınızda employee isimli sütun bilgileri id(INTEGER), name VARCHAR(50), birthday DATE, email VARCHAR(100) olan bir tablo oluşturalım.
Oluşturduğumuz employee tablosuna 'Mockaroo' servisini kullanarak 50 adet veri ekleyelim.
Sütunların her birine göre diğer sütunları güncelleyecek 5 adet UPDATE işlemi yapalım.
Sütunların her birine göre ilgili satırı silecek 5 adet DELETE işlemi yapalım.

1) Tabloyu oluşturma
DROP TABLE IF EXISTS employee;
CREATE TABLE employee (
  id       INTEGER,
  name     VARCHAR(50),
  birthday DATE,
  email    VARCHAR(100)
);

2) 50 adet veri ekleme (Mockaroo’dan üretilmiş örnek set)
INSERT INTO employee (id, name, birthday, email) VALUES
(1,  'Alice Johnson',       '1990-05-12', 'alice.johnson@gmail.com'),
(2,  'Bob Smith',           '1988-11-23', 'bob.smith@yahoo.com'),
(3,  'Carol Anderson',      '1995-02-14', 'carol.anderson@outlook.com'),
(4,  'David Miller',        '1979-07-30', 'david.miller@mailinator.com'),
(5,  'Eve Davis',           '2001-01-05', 'eve_davis@gmail.com'),
(6,  'Frank Wilson',        '1999-09-09', 'frank.wilson@yahoo.com'),
(7,  'Grace Lee',           '2003-04-18', 'grace.lee@example.com'),
(8,  'Henry Brown',         '1968-12-01', 'henry.brown@company.com'),
(9,  'Irene Taylor',        '1983-06-22', 'irene.taylor@mailinator.com'),
(10, 'Jack Thomas',         '1992-03-10', 'jack.thomas@outlook.com'),
(11, 'Karen Harris',        '1985-01-15', NULL),
(12, 'Larry Martin',        '2002-08-08', NULL),
(13, 'Monica Clark',        '1998-02-02', 'monica.clark@gmail.com'),
(14, 'Nathan Lewis',        '2004-09-19', 'nathan.lewis@yahoo.com'),
(15, 'Olivia Walker',       '1975-11-11', 'olivia.walker@company.com'),
(16, 'Peter Hall',          '1990-01-20', 'peter.hall@mailinator.com'),
(17, 'Queenie Allen',       '1989-05-05', 'queenie.allen@gmail.com'),
(18, 'Roger Young',         '1997-07-07', 'roger.young@outlook.com'),
(19, 'Susan King',          '1965-04-04', 'susan.king@mailinator.com'),
(20, 'Thomas Wright',       '1993-12-12', 'thomas.wright@gmail.com'),
(21, 'Uma Scott',           '2000-02-29', 'uma.scott@gmail.com'),
(22, 'Victor Green',        '2001-10-10', 'victor.green@yahoo.com'),
(23, 'Wendy Baker',         '1982-08-18', 'wendy.baker@company.com'),
(24, 'Xavier Adams',        '1970-01-01', 'xavier.adams@mailinator.com'),
(25, 'Yolanda Nelson',      '1996-06-16', 'yolanda.nelson@gmail.com'),
(26, 'Zachary Carter',      '1991-09-21', 'zach.carter@yahoo.com'),
(27, 'Aaron Johnson',       '1988-03-03', 'aaron.johnson@company.com'),
(28, 'Brandon Peterson',    '1999-12-31', 'brandon.peterson@mailinator.com'),
(29, 'Cynthia Robinson',    '2003-07-13', 'cynthia.robinson@gmail.com'),
(30, 'Dylan Richardson',    '1984-04-17', 'dylan.richardson@outlook.com'),
(31, 'Ethan Henderson',     '1978-08-08', 'ethan.henderson@yahoo.com'),
(32, 'Fiona Coleman',       '1995-05-25', 'fiona.coleman@mailinator.com'),
(33, 'Gavin Graham',        '1969-09-02', 'gavin.graham@gmail.com'),
(34, 'Hannah Patterson',    '1994-02-20', 'hannah.patterson@company.com'),
(35, 'Ian Simmons',         '1987-12-05', 'ian.simmons@yahoo.com'),
(36, 'Julia Hughes',        '1990-06-06', 'julia.hughes@mailinator.com'),
(37, 'Kevin Bryant',        '2005-03-15', 'kevin.bryant@gmail.com'),
(38, 'Laura Foster',        '1981-01-23', 'laura.foster@outlook.com'),
(39, 'Michael Morgan',      '1976-07-29', 'michael.morgan@mailinator.com'),
(40, 'Nora Rogers',         '1992-09-09', 'nora.rogers@gmail.com'),
(41, 'Oscar Reed',          '2002-12-24', 'oscar.reed@yahoo.com'),
(42, 'Paula Cook',          '1980-05-19', 'paula.cook@company.com'),
(43, 'Quentin Powell',      '1963-03-03', 'quentin.powell@mailinator.com'),
(44, 'Rachel Long',         '1997-10-10', 'rachel.long@gmail.com'),
(45, 'Steven Jenkins',      '1999-01-09', 'steven.jenkins@mailinator.com'),
(46, 'Teresa Perry',        '1983-11-30', 'teresa.perry@outlook.com'),
(47, 'Ulysses Howard',      '1972-02-02', 'ulysses.howard@yahoo.com'),
(48, 'Vanessa Ward',        '1996-06-06', 'vanessa.ward@gmail.com'),
(49, 'Walter Cox',          '1964-04-14', 'walter.cox@mailinator.com'),
(50, 'Test User',           '1900-01-01', 'test.user@mailinator.com');

3) 5 adet UPDATE (her birinde farklı sütunu koşul alıp diğer sütun(lar)ı güncelle)
-- U1) id koşulu → email güncelle
UPDATE employee
SET email = CASE
              WHEN email IS NULL THEN NULL
              ELSE regexp_replace(email, '@.*$', '@acme.com')
            END
WHERE id BETWEEN 1 AND 10;

-- U2) name koşulu → birthday güncelle
UPDATE employee
SET birthday = DATE '1985-01-01'
WHERE name ILIKE '%son';

-- U3) birthday koşulu → email güncelle
UPDATE employee
SET email = split_part(email,'@',1) || '.jr@' || split_part(email,'@',2)
WHERE birthday >= DATE '2000-01-01'
  AND email IS NOT NULL;

-- U4) email koşulu → name güncelle
UPDATE employee
SET name = name || ' (Y)'
WHERE email ILIKE '%@yahoo.com';

-- U5) email NULL koşulu → email üret (name’den)
UPDATE employee
SET email = regexp_replace(lower(name), '[^a-z0-9]+', '.', 'g') || '@company.com'
WHERE email IS NULL;

4) 5 adet DELETE (her birinde farklı sütunu koşul alarak satır sil)
-- D1) id koşulu
DELETE FROM employee
WHERE id = 46;

-- D2) name koşulu
DELETE FROM employee
WHERE name ILIKE '%test%';

-- D3) birthday koşulu
DELETE FROM employee
WHERE birthday < DATE '1960-01-01'
   OR birthday IS NULL;

-- D4) email NULL/boş koşulu
DELETE FROM employee
WHERE email IS NULL OR email = '';

-- D5) email domain koşulu
DELETE FROM employee
WHERE email ILIKE '%@mailinator.com';
