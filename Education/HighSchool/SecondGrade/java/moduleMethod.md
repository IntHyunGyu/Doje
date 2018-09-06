# 제목 :  모듈화 적용시켜보자!

#### getWebtoonInfo() 함수 : 제일 먼저 실행되는 함수

~~~java
/*
	 * 파라미터 없음
	 * WebtoonVO 형 ArrayList 반환
	 * select() 메소드를 실행시킨 뒤 받은 리턴값을 ArrayList에 저장 후 Jsp에 전달
	 */
	public List<WebtoonVO> getWebtoonInfo() throws IllegalArgumentException, IllegalAccessException {
		
		List<WebtoonVO> list = new ArrayList<WebtoonVO>();
		try {
			list = select();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		if(list == null) {	//검색된 값이 없을 때 즉, DB에 아무런 값이 없을 때 실행
			updateWebtoon();
		}
		System.out.println("실행중");
		return list;
	}
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
/*
	 * ResultSet 객체를 파라미터로 받음
	 * rs.next()메소드를 이용하여 추가한 list 반환
	 * rs.next()가 false가 될 때까지 getResultSet()메소드에 ResultSet객체를 실어서 실행시킴
	 */
	public List<WebtoonVO> whileRs(ResultSet rs) throws IllegalArgumentException, IllegalAccessException {
		List<WebtoonVO> list = new ArrayList<WebtoonVO>();

		try {
			while(rs.next()) {
				list.add(getReusltSet(rs));	//검색된 값의 수만큼 VO객체에 저장하는 함수 실행하고 list에 추
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return list;
	}
~~~



간단하게 list객체에 값들을 add시키는 기능을 하는 함수

---

#### getResultSet(rs) 함수 : 네번째로 실행되는 함수( 마지막 단계이며 rs객체를 파라미터로 받음 )

~~~java
/*
	 * ResultSet 객체를 파라미터로 받음
	 * ob라는 WebtoonVO형 객체를 생성후 필드에 값을 set시켜서 반환함
	 * WebtoonVO클래스 안에 있는 변수 목록들을 가져와 DB검색 값이 담겨있는 파라미터 rs를 set시킨 후 반환시킨다.
	 */
	public WebtoonVO getReusltSet(ResultSet rs) throws SQLException, IllegalArgumentException, IllegalAccessException {
		WebtoonVO ob = new WebtoonVO();
		for(Field f : ob.getClass().getDeclaredFields()) { // WebtoonVO의 필드 변수들을 가져온다.
			f.setAccessible(true);	// private 변수들을 액세스 하기 위한 함수인 것 같다.
			String name = f.getName();	//name변수에 가져온 필드 변수 명들을 저장한다.
			f.set(ob, rs.getString(name));	//ob 변수들에 검색된 데이터들을 set시
		}

		return ob;	//set된 ob 객체 반환
	}
~~~



getResultSet함수는 데이터베이스에서 검색한 값들을 VO에 세팅시켜주는 함수이다.

getClass함수와 getDeclaredFields함수를 이용하여 vo클래스의 필드 변수명들을 쉽게 가져올 수 있다.

세팅된 vo를 반환시켜 whileRs 함수에서 리스트에 추가할 수 있게함.

---



추신 : 늦게 올려서 죄송합니다. 프로젝트 완전히 정상적으로 돌리게 하는 데 시간 꽤 걸렸네요ㅠㅠ



