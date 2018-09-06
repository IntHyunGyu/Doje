# 제목 :  모듈화 적용시켜보자!

#### getWebtoonInfo() 함수 : 제일 먼저 실행되는 함수

~~~java

~~~



List 객체를 jsp로 보냄

---

#### select() 함수 : 두번째로 실행되는 함수

~~~java
/*
	 * 파라미터 값 없음
	 *  WebtoonVO 형 list 반환
	 * While() 메소드에 ResultSet객체를 담아서 실행 후 반환 된 list를 반환
*/
	public List<WebtoonVO> select() throws SQLException, IllegalArgumentException, IllegalAccessException {
		System.out.println("start");
		String tableName = "webtoon";
		String whereSql = "";
		String sql = "select * from " + tableName + " ORDER BY week";
		
		Connection conn = JdbcUtil.getConnection();
		
			ResultSet rs = null;
			PreparedStatement pstmt = null;
			pstmt = conn.prepareStatement(sql);
			rs = pstmt.executeQuery();
						
		return whileRs(rs);	//list로 반환
	}
~~~



select() 함수에서 sql쿼리문을 DB에 날리고 받은 결과값을 ResultSet객체에 담아 WhileRs()함수에 실어보냄

---

#### whileRs(rs) 함수 : 세번째로 실행되는 함수( rs변수를 파라미터로 받음 )

~~~java

~~~



간단하게 list객체에 값들을 add시키는 기능을 하는 함수

---

#### getResultSet(rs) 함수 : 네번째로 실행되는 함수( 마지막 단계이며 rs객체를 파라미터로 받음 )

~~~java

~~~



getResultSet함수는 데이터베이스에서 검색한 값들을 VO에 세팅시켜주는 함수이다.

getClass함수와 getDeclaredFields함수를 이용하여 vo클래스의 필드 변수명들을 쉽게 가져올 수 있다.

세팅된 vo를 반환시켜 whileRs 함수에서 리스트에 추가할 수 있게함.

---



추신 : 늦게 올려서 죄송합니다. 프로젝트 완전히 정상적으로 돌리게 하는 데 시간 꽤 걸렸네요ㅠㅠ



