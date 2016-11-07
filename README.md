+ <? XML версия = "1.0" кодирование = "UTF-8"?>
+
+ <История>
+
+ <HistoryItem>
+
+ <SQL> <[CDATA [переменная REFCUR1 REFCURSOR!;
+ Переменная REFCUR2 REFCURSOR;
+ Переменная REFCUR3 REFCURSOR;
+ Переменная REFCUR4 REFCURSOR;
+ Переменная REFCUR5 REFCURSOR;
+
+ DECLARE
+
+ StrCOMPCD VARCHAR2 (4): = '3061'; 
+ StrAREAID VARCHAR2 (2): = ''; 
+ StrBEYYMM УАКСНАК (7): = '2015-01'; 
+ StrEDYYMM УАКСНАК (7): = '2015-01'; 
+ StrPYYEAR VARCHAR (4): = '2015'; 
+ StrPYTERM УАКСНАК (2): = '01'; 
+ StrPYGUBN VARCHAR (1): = '1'; 
+ StrBONUID VARCHAR (4): = ''; 
+ StrTABLE VARCHAR (6): = 'PY1002'; 
+ StrORGAID VARCHAR (8): = ''; 
+ StrLANGCD УАКСНАК (2): = 'КО'; 
+ StrEMPNUM УАКСНАК (8): = '200007'; 
+ StrWORKCD VARCHAR (4): = ''; 
+ StrCOMPID VARCHAR (4): = ''; 

 	strCOMPID VARCHAR (4): = ''; 		 	strCOMPID VARCHAR (4): = ''; 
 	strEMPNUM_Search УАКСНАК (10): = '200007'; 		 	strEMPNUM_Search УАКСНАК (10): = '200007'; 
 	strPYDATE VARCHAR2 (10); 			 	strPYDATE VARCHAR2 (10); 	
+ SQLSelect VARCHAR2 (4000);
+ SqlBasis VARCHAR2 (4000);
+ ColSelect VARCHAR2 (100);
+ ColBasis VARCHAR2 (100);
+ Colop VARCHAR2 (100);
+ BasisType VARCHAR2 (1);
+ StrDFDATE VARCHAR2 (10);
+ StrAUTHORGA VARCHAR2 (8);
+ StrRTORGA VARCHAR2 (8);
+ StrPLANVR VARCHAR (2);
+ StrSQL VARCHAR2 (4000);
+ V_stmt varchar2 (4000);
+
+ НАЧАТЬ
+
+ ЕСЛИ strAREAID <> '' ТОГДА
+	 
+ ВЫБОР PYEDDT в strDFDATE FROM PY0003 WHERE COMPCD = strCOMPCD И Areaid = strAREAID И PYYEAR = strPYYEAR И PYTERM = strPYTERM;
+	 
+ ELSE
+ 
+ ВЫБОР to_char (ADD_MONTHS (to_date (strPYYEAR || '-' || strPYTERM || '-01', 'YYYY-MM-DD'), 1) -1, 'YYYY-MM-DD') в strDFDATE FROM DUAL ;
+	
+ END IF;
+ DBMS_OUTPUT.PUT_LINE ( ​​'111111111111');
+ - // 하기 의 변수 사용 하는 곳 없음 2015.06.30 
+ ВЫБОР GetEmpRootORGAIDByAuth (strCOMPCD, strEMPNUM, to_char (SYSDATE, 'YYYY-MM-DD')) в strAUTHORGA от двойного;
+
+ ВЫБОР OBJTID в strRTORGA FROM ST0000 WHERE COMPCD = strCOMPCD;
+
+ StrPLANVR: = '01';
+ DBMS_OUTPUT.PUT_LINE ( ​​'2222222222222222');
+	 
+ SQLSelect: = '
+ ВЫБОР A.EMPNUM, A.PYYEAR, A.PYTERM, A.PYDATE, A.PYGUBN, A.BONUID, A.WGTYPE, A.PYAMOT
+ ОТ
+ '|| strTABLE || 'A
+ LEFT OUTER JOIN PA0002 B ON A.COMPCD = B.COMPCD И A.EMPNUM = B.EMPNUM И B.SBTYPE = '' 0001 '' И B.BEDATE <= '' '|| strDFDATE || '' 'И B.EDDATE> =' '' || strDFDATE || '' '
+ LEFT OUTER JOIN PYT9003 C ON A.COMPCD = C.COMPCD И B.WORKCD = C.WORKCD И C.BEDATE <= '' '|| strDFDATE || '' 'И C.EDDATE> =' '' || strDFDATE || '' '
+ WHERE
+ A.COMPCD = '' '|| strCOMPCD || '' '
+ И A.AREAID = ДЕКОДИРОВАТЬ ( '' '|| strAREAID ||' '', '' '', A.AREAID, '' '|| strAREAID ||' '')
+ И A.PYYEAR || '' - '' || A.PYTERM МЕЖДУ '' '|| strBEYYMM || '' 'И' '' || strEDYYMM || '' '
+ И A.PYGUBN = '' '|| strPYGUBN || '' '
+ И A.BONUID = ДЕКОДИРОВАТЬ ( '' '|| strBONUID ||' '', '' '', A.BONUID, '' '|| strBONUID ||' '')
+ И A.PADEGB IN ( '' Р '', '' S '', '' Z '')
+ И B.WORKCD = ДЕКОДИРОВАТЬ ( '' '|| strWORKCD ||' '', '' '', B.WORKCD, '' '|| strWORKCD ||' '')
+ И C.COMPID = ДЕКОДИРОВАТЬ ( '' '|| strCOMPID ||' '', '' '', C.COMPID, '' '|| strCOMPID ||' '')
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ И B.ORGAID = ДЕКОДИРОВАТЬ ( '' '|| strORGAID ||' '', '' '', B.ORGAID, '' '|| strORGAID ||' '')
+ И A.PYDATE = ДЕКОДИРОВАТЬ ( '' '|| strPYDATE ||' '', '' '', A.PYDATE, '' '|| strPYDATE ||' '')
+ ';
+ DBMS_OUTPUT.PUT_LINE ( ​​'333333333333333');
+ - 총 지급액, 총 공제액, 비과세 금액 СОЮЗ
+ SQLSelect: = SQLSelect || '
+ UNION ALL
+ ВЫБОР A.EMPNUM, A.PYYEAR, A.PYTERM, A.PYDATE, A.PYGUBN, A.BONUID, CASE A.PADEGB КОГДА '' Р '', затем '' PYAMTP '', когда '' S '' тогда ' 'PYAMTS' ', когда' 'Z' ', затем' 'PYAMTT' 'END AS PADEGB, SUM (NVL (A.PYAMOT, 0))
+ ОТ
+ '|| strTABLE || 'A
+ LEFT OUTER JOIN PA0002 B ON A.COMPCD = B.COMPCD И A.EMPNUM = B.EMPNUM И B.SBTYPE = '' 0001 '' И B.BEDATE <= '' '|| strDFDATE || '' 'И B.EDDATE> =' '' || strDFDATE || '' '
+ LEFT OUTER JOIN PYT9003 C ON A.COMPCD = C.COMPCD И B.WORKCD = C.WORKCD И C.BEDATE <= '' '|| strDFDATE || '' 'И C.EDDATE> =' '' || strDFDATE || '' '
+ WHERE
+ A.COMPCD = '' '|| strCOMPCD || '' '
+ И A.AREAID = ДЕКОДИРОВАТЬ ( '' '|| strAREAID ||' '', '' '', A.AREAID, '' '|| strAREAID ||' '')
+ И A.PYYEAR || '' - '' || A.PYTERM МЕЖДУ '' '|| strBEYYMM || '' 'И' '' || strEDYYMM || '' '
+ И A.PYGUBN = '' '|| strPYGUBN || '' '
+ И A.BONUID = ДЕКОДИРОВАТЬ ( '' '|| strBONUID ||' '', '' '', A.BONUID, '' '|| strBONUID ||' '')
+ И A.PADEGB IN ( '' Р '', '' S '', '' Z '')
+ И B.WORKCD = ДЕКОДИРОВАТЬ ( '' '|| strWORKCD ||' '', '' '', B.WORKCD, '' '|| strWORKCD ||' '')
+ И C.COMPID = ДЕКОДИРОВАТЬ ( '' '|| strCOMPID ||' '', '' '', C.COMPID, '' '|| strCOMPID ||' '')
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ И B.ORGAID = ДЕКОДИРОВАТЬ ( '' '|| strORGAID ||' '', '' '', B.ORGAID, '' '|| strORGAID ||' '')
+ И A.PYDATE = ДЕКОДИРОВАТЬ ( '' '|| strPYDATE ||' '', '' '', A.PYDATE, '' '|| strPYDATE ||' '')
+ GROUP BY A.EMPNUM, A.PYYEAR, A.PYTERM, A.PYDATE, A.PYGUBN, A.BONUID, A.PADEGB
+ ';
+ DBMS_OUTPUT.PUT_LINE ( ​​'4444444444444444');
+ - 차인 지급액 СОЮЗ
+ SQLSelect: = SQLSelect || '
+ UNION ALL
+ ВЫБОР EMPNUM, PYYEAR, PYTERM, PYDATE, PYGUBN, BONUID, '' PYAMTC '', NVL (SUM (PYAMTP), 0) - NVL (SUM (PYAMTS), 0)
+ ОТ
+ (
+ ВЫБОР A.EMPNUM, A.PYYEAR, A.PYTERM, A.PYDATE, A.PYGUBN, A.BONUID, CASE A.PADEGB КОГДА '' Р '' THEN SUM (NVL (A.PYAMOT, 0)) ELSE 0 END AS '' PYAMTP '', CASE A.PADEGB КОГДА '' S '' THEN SUM (NVL (A.PYAMOT, 0)) END ELSE 0 AS '' PYAMTS ''
+ ОТ
+ '|| strTABLE || 'A
+ LEFT OUTER JOIN PA0002 B ON A.COMPCD = B.COMPCD И A.EMPNUM = B.EMPNUM И B.SBTYPE = '' 0001 '' И B.BEDATE <= '' '|| strDFDATE || '' 'И B.EDDATE> =' '' || strDFDATE || '' '
+ LEFT OUTER JOIN PYT9003 C ON A.COMPCD = C.COMPCD И B.WORKCD = C.WORKCD И C.BEDATE <= '' '|| strDFDATE || '' 'И C.EDDATE> =' '' || strDFDATE || '' '
+ WHERE
+ A.COMPCD = '' '|| strCOMPCD || '' '
+ И A.AREAID = ДЕКОДИРОВАТЬ ( '' '|| strAREAID ||' '', '' '', A.AREAID, '' '|| strAREAID ||' '')
+ И A.PYYEAR || '' - '' || A.PYTERM МЕЖДУ '' '|| strBEYYMM || '' 'И' '' || strEDYYMM || '' '
+ И A.PYGUBN = '' '|| strPYGUBN || '' '
+ И A.BONUID = ДЕКОДИРОВАТЬ ( '' '|| strBONUID ||' '', '' '', A.BONUID, '' '|| strBONUID ||' '')
+ И A.PADEGB IN ( '' Р '', '' S '')
+ И B.WORKCD = ДЕКОДИРОВАТЬ ( '' '|| strWORKCD ||' '', '' '', B.WORKCD, '' '|| strWORKCD ||' '')
+ И C.COMPID = ДЕКОДИРОВАТЬ ( '' '|| strCOMPID ||' '', '' '', C.COMPID, '' '|| strCOMPID ||' '')
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ И B.ORGAID = ДЕКОДИРОВАТЬ ( '' '|| strORGAID ||' '', '' '', B.ORGAID, '' '|| strORGAID ||' '')
+ И A.PYDATE = ДЕКОДИРОВАТЬ ( '' '|| strPYDATE ||' '', '' '', A.PYDATE, '' '|| strPYDATE ||' '')
+ GROUP BY A.EMPNUM, A.PYYEAR, A.PYTERM, A.PADEGB, A.PYDATE, A.PYGUBN, A.BONUID
+) A
+ GROUP BY EMPNUM, PYYEAR, PYTERM, PYDATE, PYGUBN, BONUID
+ ';
+ DBMS_OUTPUT.PUT_LINE ( ​​'555555555555555555 =>' || Длина (SQLSelect));
+ - 과세 금액 СОЮЗ
+  
+ SQLSelect: = SQLSelect || '
+ UNION ALL
+ ВЫБОР EMPNUM, PYYEAR, PYTERM, PYDATE, PYGUBN, BONUID, '' PYAMTX '', NVL (SUM (PYAMTP), 0) - NVL (SUM (PYAMTT), 0)
+ ОТ
+ (
+ ВЫБОР A.EMPNUM, A.PYYEAR, A.PYTERM, A.PYDATE, A.PYGUBN, A.BONUID, CASE A.PADEGB КОГДА '' Р '' THEN SUM (NVL (A.PYAMOT, 0)) ELSE 0 END AS '' PYAMTP '', CASE A.PADEGB, когда '' Z '' просуммировать (NVL (A.PYAMOT, 0)) 0 ELSE END AS '' PYAMTT ''
+ ОТ
+ '|| strTABLE || 'A
+ LEFT OUTER JOIN PA0002 B ON A.COMPCD = B.COMPCD И A.EMPNUM = B.EMPNUM И B.SBTYPE = '' 0001 '' И B.BEDATE <= '' '|| strDFDATE || '' 'И B.EDDATE> =' '' || strDFDATE || '' '
+ LEFT OUTER JOIN PYT9003 C ON A.COMPCD = C.COMPCD И B.WORKCD = C.WORKCD И C.BEDATE <= '' '|| strDFDATE || '' 'И C.EDDATE> =' '' || strDFDATE || '' '
+ WHERE
+ A.COMPCD = '' '|| strCOMPCD || '' '
+ И A.AREAID = ДЕКОДИРОВАТЬ ( '' '|| strAREAID ||' '', '' '', A.AREAID, '' '|| strAREAID ||' '')
+ И A.PYYEAR || '' - '' || A.PYTERM МЕЖДУ '' '|| strBEYYMM || '' 'И' '' || strEDYYMM || '' '
+ И A.PYGUBN = '' '|| strPYGUBN || '' '
+ И A.BONUID = ДЕКОДИРОВАТЬ ( '' '|| strBONUID ||' '', '' '', A.BONUID, '' '|| strBONUID ||' '')
+ И A.PADEGB IN ( '' Р '', '' Z '')
+ И B.WORKCD = ДЕКОДИРОВАТЬ ( '' '|| strWORKCD ||' '', '' '', B.WORKCD, '' '|| strWORKCD ||' '')
+ И C.COMPID = ДЕКОДИРОВАТЬ ( '' '|| strCOMPID ||' '', '' '', C.COMPID, '' '|| strCOMPID ||' '')
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ И B.ORGAID = ДЕКОДИРОВАТЬ ( '' '|| strORGAID ||' '', '' '', B.ORGAID, '' '|| strORGAID ||' '')
+ И A.PYDATE = ДЕКОДИРОВАТЬ ( '' '|| strPYDATE ||' '', '' '', A.PYDATE, '' '|| strPYDATE ||' '')
+ GROUP BY A.EMPNUM, A.PYYEAR, A.PYTERM, A.PADEGB, A.PYDATE, A.PYGUBN, A.BONUID
+) A
+ GROUP BY EMPNUM, PYYEAR, PYTERM, PYDATE, PYGUBN, BONUID
+ ';
+ DBMS_OUTPUT.PUT_LINE ( ​​'66666666666666');
+ - 연장 근무 시간 СОЮЗ
+ SQLSelect: = SQLSelect || '
+ UNION ALL
+ ВЫБОР A.EMPNUM, B.PYYEAR, B.PYTERM, B.PYDATE, '' 1 '', '' '',
+ ДЕЛО A.WGTYPE КОГДА '' W020 '' THEN '' Ottime '', когда '' W022 '' THEN '' NTTIME '', когда '' W021 '' THEN '' SPTIME '' ELSE '' и т.д. '' END,
+ NVL (SUM (A.WKTIME), 0)
+ ОТ
+ TM3000
+ INNER JOIN PY0003 B ON A.COMPCD = B.COMPCD И A.TMDATE> = B.TMBEDT И A.TMDATE <= B.TMEDDT
+ WHERE
+ B.COMPCD = '' '|| strCOMPCD || '' '
+ И B.AREAID = ДЕКОДИРОВАТЬ ( '' '|| strAREAID ||' '', '' '', B.AREAID, '' '|| strAREAID ||' '')
+ И B.PYYEAR || '' - '' || B.PYTERM МЕЖДУ '' '|| strBEYYMM || '' 'И' '' || strEDYYMM || '' '
+ И B.COMPCD = ДЕКОДИРОВАТЬ ( '' '|| strPYGUBN ||' '', '' 1 '', B.COMPCD, '' '')
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ GROUP BY EMPNUM, B.PYYEAR, B.PYTERM, B.PYDATE, WGTYPE
+ ';
+ DBMS_OUTPUT.PUT_LINE ( ​​'77777777777777777');
+ - ROW => КОЛОННА 으로 돌릴 КОЛОННА Строка 생성
+ SqlBasis: = '
+ SELECT DISTINCT A.WGTYPE
+ ОТ
+ '|| strTABLE || 'A
+ LEFT OUTER JOIN PA0002 B ON A.COMPCD = B.COMPCD И A.EMPNUM = B.EMPNUM И B.SBTYPE = '' 0001 '' И B.BEDATE <= '' '|| strDFDATE || '' 'И B.EDDATE> =' '' || strDFDATE || '' '
+ LEFT OUTER JOIN PYT9003 C ON A.COMPCD = C.COMPCD И B.WORKCD = C.WORKCD И C.BEDATE <= '' '|| strDFDATE || '' 'И C.EDDATE> =' '' || strDFDATE || '' '
+ WHERE
+ A.COMPCD = '' '|| strCOMPCD || '' '
+ И A.AREAID = ДЕКОДИРОВАТЬ ( '' '|| strAREAID ||' '', '' '', A.AREAID, '' '|| strAREAID ||' '')
+ И A.PYYEAR || '' - '' || A.PYTERM МЕЖДУ '' '|| strBEYYMM || '' 'И' '' || strEDYYMM || '' '
+ И A.PYGUBN = '' '|| strPYGUBN || '' '
+ И A.BONUID = ДЕКОДИРОВАТЬ ( '' '|| strBONUID ||' '', '' '', A.BONUID, '' '|| strBONUID ||' '')
+ И A.PADEGB IN ( '' Р '', '' S '', '' Z '')
+ И A.WGTYPE <> '' T996 '' и WGTYPE <> '' T997 ''
+ И B.WORKCD = ДЕКОДИРОВАТЬ ( '' '|| strWORKCD ||' '', '' '', B.WORKCD, '' '|| strWORKCD ||' '')
+ И C.COMPID = ДЕКОДИРОВАТЬ ( '' '|| strCOMPID ||' '', '' '', C.COMPID, '' '|| strCOMPID ||' '')
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ И B.ORGAID = ДЕКОДИРОВАТЬ ( '' '|| strORGAID ||' '', '' '', B.ORGAID, '' '|| strORGAID ||' '')
+ И A.PYDATE = ДЕКОДИРОВАТЬ ( '' '|| strPYDATE ||' '', '' '', A.PYDATE, '' '|| strPYDATE ||' '')
+ UNION ALL
+ Выбрать '' PYAMTP '' FROM DUAL
+ UNION ALL
+ Выбрать '' PYAMTS '' FROM DUAL
+ UNION ALL 
+ Выбрать '' PYAMTT '' FROM DUAL
+ UNION ALL 
+ Выбрать '' PYAMTC '' FROM DUAL
+ UNION ALL
+ Выбрать '' PYAMTX '' FROM DUAL
+ UNION ALL
+ Выбрать '' Ottime '' FROM DUAL
+ UNION ALL
+ Выбрать '' NTTIME '' FROM DUAL
+ UNION ALL
+ Выбрать '' SPTIME '' FROM DUAL
+ ';
+
+ DBMS_OUTPUT.PUT_LINE ( ​​'+88888888888888888888888');
+ ColSelect: = 'PYAMOT';
+ ColBasis: = 'WGTYPE';
+ Colop: = 'MAX';
+ BasisType: = '';
+
+ - 사번 / 임금 유형 .... 형태 로 금액 Вернуться
+	 
+ V_stmt: = 'выберите * из (' || SQLSelect || ') ось (MAX (PYAMOT) для WGTYPE в (' || SqlBasis || '))';
+
+ OPEN: REFCUR1 ДЛЯ v_stmt;    
+
+
+
+ StrSQL: = '
+ ВЫБОР
+ A.PAYSUB, A.WGTYPE, B.WGTEXT
+ ОТ
+ PAT0012C
+ LEFT OUTER JOIN PAT0012 B ON A.COMPCD = B.COMPCD И A.WGTYPE = B.WGTYPE И B.LANGCD = strLANGCD
+ WHERE
+ A.WGTYPE IN
+ (
+ SELECT DISTINCT A.WGTYPE
+ ОТ
+ '|| strTABLE || 'A
+ LEFT OUTER JOIN PA0002 B ON A.COMPCD = B.COMPCD И A.EMPNUM = B.EMPNUM И B.SBTYPE = '' 0001 '' И B.BEDATE <= '' '|| strDFDATE || '' 'И B.EDDATE> =' '' || strDFDATE || '' '
+ LEFT OUTER JOIN PYT9003 C ON A.COMPCD = C.COMPCD И B.WORKCD = C.WORKCD И C.BEDATE <= '' '|| strDFDATE || '' 'И C.EDDATE> =' '' || strDFDATE || '' '
+ WHERE
+ A.COMPCD = strCOMPCD
+ И A.AREAID = ДЕКОДИРОВАТЬ (strAREAID, '' '', A.AREAID, strAREAID)
+ И A.PYYEAR || '' - '' || A.PYTERM МЕЖДУ strBEYYMM И strEDYYMM
+ И A.PYGUBN = strPYGUBN
+ И A.BONUID = ДЕКОДИРОВАТЬ (strBONUID, '' '', A.BONUID, strBONUID)
+ И A.PADEGB IN ( '' Р '')
+ И B.WORKCD = ДЕКОДИРОВАТЬ (strWORKCD, '' '', B.WORKCD, strWORKCD)
+ И C.COMPID = ДЕКОДИРОВАТЬ (strCOMPID, '' '', C.COMPID, strCOMPID)
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ И B.ORGAID = ДЕКОДИРОВАТЬ ( '' '|| strORGAID ||' '', '' '', B.ORGAID, '' '|| strORGAID ||' '')
+ И A.PYDATE = ДЕКОДИРОВАТЬ ( '' '|| strPYDATE ||' '', '' '', A.PYDATE, '' '|| strPYDATE ||' '')
+)
+ ORDER BY PAYSUB, PRTSEQ
+ ';
+   
+
+ - 급 / 상여 조회 임금 유형 Заголовок 정보 Выберите
+ OPEN: REFCUR2 ДЛЯ strSQL;
+ 
+					
+ StrSQL: = '
+ ВЫБОР
+ A.PAYSUB, A.WGTYPE, B.WGTEXT
+ ОТ
+ PAT0012C
+ LEFT OUTER JOIN PAT0012 B ON A.COMPCD = B.COMPCD И A.WGTYPE = B.WGTYPE И B.LANGCD = strLANGCD
+ WHERE
+ A.WGTYPE IN
+ (
+ SELECT DISTINCT A.WGTYPE
+ ОТ
+ '|| strTABLE || 'A
+ LEFT OUTER JOIN PA0002 B ON A.COMPCD = B.COMPCD И A.EMPNUM = B.EMPNUM И B.SBTYPE = '' 0001 '' И B.BEDATE <= '' '|| strDFDATE || '' 'И B.EDDATE> =' '' || strDFDATE || '' '
+ LEFT OUTER JOIN PYT9003 C ON A.COMPCD = C.COMPCD И B.WORKCD = C.WORKCD И C.BEDATE <= '' '|| strDFDATE || '' 'И C.EDDATE> =' '' || strDFDATE || '' '
+ WHERE
+ A.COMPCD = strCOMPCD
+ И A.AREAID = ДЕКОДИРОВАТЬ (strAREAID, '' '', A.AREAID, strAREAID)
+ И A.PYYEAR || '' - '' || A.PYTERM МЕЖДУ strBEYYMM И strEDYYMM
+ И A.PYGUBN = strPYGUBN
+ И A.BONUID = ДЕКОДИРОВАТЬ (strBONUID, '' '', A.BONUID, strBONUID)
+ И A.PADEGB IN ( '' S '')
+ И B.WORKCD = ДЕКОДИРОВАТЬ (strWORKCD, '' '', B.WORKCD, strWORKCD)
+ И C.COMPID = ДЕКОДИРОВАТЬ (strCOMPID, '' '', C.COMPID, strCOMPID)
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ И B.ORGAID = ДЕКОДИРОВАТЬ ( '' '|| strORGAID ||' '', '' '', B.ORGAID, '' '|| strORGAID ||' '')
+ И A.PYDATE = ДЕКОДИРОВАТЬ ( '' '|| strPYDATE ||' '', '' '', A.PYDATE, '' '|| strPYDATE ||' '')
+)
+ ORDER BY PAYSUB, PRTSEQ
+ ';
+
+ OPEN: REFCUR3 ДЛЯ strSQL;
+
+ 
+ StrSQL: = '
+ ВЫБОР
+ A.PAYSUB, A.WGTYPE, B.WGTEXT
+ ОТ
+ PAT0012C
+ LEFT OUTER JOIN PAT0012 B ON A.COMPCD = B.COMPCD И A.WGTYPE = B.WGTYPE И B.LANGCD = strLANGCD
+ WHERE
+ A.WGTYPE IN
+ (
+ SELECT DISTINCT A.WGTYPE
+ ОТ
+ '|| strTABLE || 'A
+ LEFT OUTER JOIN PA0002 B ON A.COMPCD = B.COMPCD И A.EMPNUM = B.EMPNUM И B.SBTYPE = '' 0001 '' И B.BEDATE <= '' '|| strDFDATE || '' 'И B.EDDATE> =' '' || strDFDATE || '' '
+ LEFT OUTER JOIN PYT9003 C ON A.COMPCD = C.COMPCD И B.WORKCD = C.WORKCD И C.BEDATE <= '' '|| strDFDATE || '' 'И C.EDDATE> =' '' || strDFDATE || '' '
+ WHERE
+ A.COMPCD = strCOMPCD
+ И A.AREAID = ДЕКОДИРОВАТЬ (strAREAID, '' '', A.AREAID, strAREAID)
+ И A.PYYEAR || '' - '' || A.PYTERM МЕЖДУ strBEYYMM И strEDYYMM
+ И A.PYGUBN = strPYGUBN
+ И A.BONUID = ДЕКОДИРОВАТЬ (strBONUID, '' '', A.BONUID, strBONUID)
+ И A.PADEGB IN ( '' Z '')
+ И B.WORKCD = ДЕКОДИРОВАТЬ (strWORKCD, '' '', B.WORKCD, strWORKCD)
+ И C.COMPID = ДЕКОДИРОВАТЬ (strCOMPID, '' '', C.COMPID, strCOMPID)
+ И A.EMPNUM = ДЕКОДИРОВАТЬ ( '' '|| strEMPNUM_Search ||' '', '' '', A.EMPNUM, '' '|| strEMPNUM_Search ||' '')
+ И B.ORGAID = ДЕКОДИРОВАТЬ ( '' '|| strORGAID ||' '', '' '', B.ORGAID, '' '|| strORGAID ||' '')
+ И A.PYDATE = ДЕКОДИРОВАТЬ ( '' '|| strPYDATE ||' '', '' '', A.PYDATE ELSE '' '|| strPYDATE ||' '')
+)
+ ORDER BY PAYSUB, PRTSEQ
+ ';
+
+ OPEN: REFCUR4 ДЛЯ strSQL;
+
+
+  
+ - 급 / 상여 조회 사원 정보 Выберите
+ HR_PYMD_PY6000A_SelectEmp (strCOMPCD, strAREAID, strBEYYMM, strEDYYMM, strPYYEAR, strPYTERM, strPYGUBN, strBONUID, strTABLE, strORGAID, strLANGCD, 
+ StrEMPNUM, strWORKCD, strCOMPID, strEMPNUM_Search, strPYDATE,: REFCUR5);
+
+ END;]]> </ SQL>
+
+ <Соединение> <! [CDATA [ШР]]> </ связь>
+
+ <Метка времени> <! [CDATA [1443683454583]]> </ метка времени>
+
+ <Тип> <! [CDATA [Script]]> </ тип>
+
+ <Казнены> <! [CDATA [1]]> </ казнены>
+
+ <ExecTime> <! [CDATA [0,223]]> </ execTime>
+
+ </ HistoryItem>
+
+ </ История>
+
+ <? XML версия = "1.0" кодирование = "UTF-8"?>
+
+ <История>
+
+ <HistoryItem>
+
+ <SQL> <[CDATA [переменная REFCUR1 REFCURSOR!;
+ Переменная REFCUR2 REFCURSOR;
+ Переменная REFCUR3 REFCURSOR;
+ Переменная REFCUR4 REFCURSOR;
+ Переменная REFCUR5 REFCURSOR;
+
+ DECLARE
+
+ StrCOMPCD VARCHAR2 (4): = '3061'; 
+ StrAREAID VARCHAR2 (2): = ''; 
+ StrBEYYMM УАКСНАК (7): = '2015-01'; 
+ StrEDYYMM УАКСНАК (7): = '2015-01'; 
+ StrPYYEAR VARCHAR (4): = '2015'; 
+ StrPYTERM УАКСНАК (2): = '01'; 
+ StrPYGUBN VARCHAR (1): = '1'; 
+ StrBONUID VARCHAR (4): = ''; 
+ StrTABLE VARCHAR (6): = 'PY1002'; 
+ StrORGAID VARCHAR (8): = ''; 
+ StrLANGCD УАКСНАК (2): = 'КО'; 
+ StrEMPNUM УАКСНАК (8): = '200007'; 
+ StrWORKCD VARCHAR (4): = ''; 
+ StrCOMPID VARCHAR (4): = ''; 
+ StrEMPNUM_Search УАКСНАК (10): = '200007'; 
+ StrPYDATE VARCHAR2 (10); 	
     SQLSelect VARCHAR2 (32000);		     SQLSelect VARCHAR2 (32000);
 	SqlBasis VARCHAR2 (4000);		 	SqlBasis VARCHAR2 (4000);
 	ColSelect VARCHAR2 (100);		 	ColSelect VARCHAR2 (100);
