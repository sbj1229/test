package PBL3;

/* 작성자는 주석으로 주요 수정 내용을 작성/수정시에 기술바람
 * 수정내역 ======
 * 5/30 손범준
 * 학생에 학점(신청가능 학점) 필드 추가
 * 사람에 pw(로그인용) 필드 추가
 * 학생에 장학금 내부클래스로 변경(사용 테스트 필요)
 * 사람-직원들 사이에 admin 중간클래스 추가
 * 
 * ============
 * 6/1 김현우
 * Person클래스에 교수, 학생, 조교, 직원 숫자를 나타내는 static int형 추가.
 * 
 * ================
 * 6/3 김현우
 * subject 클래스 수정.
 * 
 * 학생,교수,직원,조교 작성 영역
 * ================
 * 6/4 정재훈
 * 타입을 전부 String으로 바꾸고
 * 상의하고 변경한 부분을 수정
 * 아래쪽에 Person[] std = new Person[maxStd]; 부분 수정요청
 * 
 * 그외 추가로 필요한것 같은 기능 / 위의 기능중 완료한 기능은 아래에 작성바람
 * 완성한기능( ex) 추가기능 : Add_Person() )
 * 
 * 필요한것 같은 기능
 * 
 * 만든 기능에 가급적이면 짧게라도 주석으로 설명을 기술하는것을 권장
 */
interface Rank {
	double Ap = 4.5, Az = 4.30, Am = 4.0;
	double Bp = 3.5, Bz = 3.30, Bm = 3.0;
	double Cp = 2.5, Cz = 2.30, Cm = 2.0;
	double Dp = 1.5, Dz = 1.30, Dm = 1.0;
	double F = 0.0;
}

class Person implements Rank {

	// 각 생성자를 호출할때마다 1씩 증가//
	static int pNum = 0; // 교수 숫자
	static int aNum = 0; // 조교 숫자
	static int stdNum = 0; // 학생 숫자
	static int sNum = 0; // 직원 숫자

	//사용자 입력값
	String ID; //학생 8자리, 교수 10자리, 조교 6자리, 직원 7자리
	String phoneNumber;
	String email;
	String name;
	String department;
	
	//초기 기본 세팅값
	String PW;	 // 기본은 id 뒷자리4개 수정시 4~8자리
	String joinDay; // 입사일
	String OutDay = "b"; // a 자퇴 b 재학 c 졸업 d 퇴직

    
	public Person(String personalNum, String phoneNumber, String officeNumber, String email, String name,
			String department) {
		this.ID = personalNum;
		this.phoneNumber = phoneNumber;
		this.email = email;
		this.name = name;
		this.department = department;
        this.PW = personalNum.substring(personalNum.length()-4);
	}
    	//패스워드 입력 생성자 ,   파일 로딩시 or 메뉴실행중 학생 추가시
	public Person(String personalNum, String phoneNumber, String email, String name, String department, String pw) {
		this.ID = personalNum;
		this.phoneNumber = phoneNumber;
		this.email = email;
		this.name = name;
		this.department = department;
		this.PW = pw;
	}
}

class Student extends Person {
	//주석 필드는 다른 클래스에서 학번기반 매칭 후 값 계산으로 
	// String subject;// 수강과목 여러개이므로 배열? 필요 예상
	// char score; // 수강과목 내부에 있어야 할듯 고ㅏㅁ
	// int credit; // 학점, 수강신청시 계산하도록? => 5.31 김현우: char배열(?)
	// int seosonCredit; //5.31 김현우: 현재학기 성적
	// int allCredit; //5.31 김현우: 전체학기 성적
	// int pastCredit; //5.31 김현우: 지난학기 성적(배열 OR 파일입출력)
	String tuition; // 등록금

	public Student(String personalNum, String phoneNumber, String email, String name, String department, 
			String tuition) {
		super(personalNum, phoneNumber, email, name, department);
		this.tuition = tuition;
	    this.joinDay = personalNum.substring(0, 4) + ".3.2";
		stdNum++;
	}
	public Student(String personalNum, String phoneNumber, String email, String name, String department, 
			String tuition, String joinDay) {
		super(personalNum, phoneNumber, email, name, department);
		this.tuition = tuition;
	    this.joinDay = joinDay;
		stdNum++;
	}
}

class Scholarship {// 해당 학기의 등록금을 장학금이 넘지 못하도록 구현 필요
	String ID;
	String name;// 장학금명 //5.30 김현우: 장학금은 여러개이므로 배열필요 A. 내부클래스이므로 배열 필요 없이
	String money; // 금액 ParseInt //5.31 김현우: 등록금, 현재학기 장학금, 전체학기 장학금 + 이전학기 기록검색에 장학금이 나와야 하므로
					// 이전학기 장학금 =>필요 => 장학금 클래스 따로 생성해야할듯 OR 파일입출력(?)
	String semester; // 수혜학기
    
    public Scholarship(String ID, String name, String money, String semester){
        this.ID=ID;
        this.name=name;
        this.money=money;
        this.semester=semester;
    }
}

class Professor extends Person {
	String salary;
	//String subject; //맡은 과목 여러개이므로 열린과목 클래스의 proId값과 매칭해서 검사
	String position;
	String officeNumber;


	// 강의과목이 없는경우//
	public Professor(String personalNum, String phoneNumber, String officeNumber, String email, String name,
			String department, String salary, String position, String joinDay) {
		super(personalNum, phoneNumber, email, name, department);
		this.salary = salary;
		this.position = position;
		this.joinDay = joinDay;
		this.officeNumber = officeNumber;
		pNum++;
	}
    	public Professor(String personalNum, String phoneNumber, String officeNumber, String email, String name,
			String department, String salary, String position, String joinDay,String pw) {
		super(personalNum, phoneNumber, email, name, department,pw);
		this.salary = salary;
		this.position = position;
		this.joinDay = joinDay;
		this.officeNumber = officeNumber;
		pNum++;
	}
		
}

class Admin extends Person {

	String salary;
	String officeNumber;
	public Admin(String personalNum, String phoneNumber, String officeNumber, String email, String name, String department, String salary,
			String joinDay) {
		super(personalNum, phoneNumber, email, name, department);
		this.salary = salary;
		this.joinDay = joinDay;
		this.officeNumber = officeNumber;

	}

}

class Staff extends Admin {
	String position;


	public Staff(String personalNum, String phoneNumber, String officeNumber, String email, String name, String department, String salary,
			String position, String joinDay) {
		super(personalNum, phoneNumber,officeNumber, email, name, department, salary, joinDay);
		this.position = position;
		sNum++;

	}
}

class Assistant extends Admin {


	public Assistant(String personalNum, String phoneNumber, String officeNumber, String email, String name, String department,
			String salary, String joinDay) {
		super(personalNum, phoneNumber,officeNumber, email, name, department, salary, joinDay);
		aNum++;
	}
}

class Department {
	String department; // 학과명
	String address; // 학과주소
	String Number; // 학과 전화번호
	String assistantName; // 학과 조교이름
	String capName; // 학과장 이름
	String Email; // 학과 이메일
	String Web; // 학과 웹사이트

	public Department(String department, String address, String Number, String assistantName, String capName,
			String Email, String Web) {
		this.department = department;
		this.address = address;
		this.Number = Number;
		this.assistantName = assistantName;
		this.capName = capName;
		this.Email = Email;
		this.Web = Web;
	}
	public Department() {}
	public void print() {
		System.out.println("학과명: " + this.department);
		System.out.println("학과주소: " + this.address);
	}

	// 학과검색//
	/*
	 * public String getDepartment() { return this.department; }
	 */

}

class Subject {
	/* 교과목 검색은 과목명, 학수번호, 개설학과로 검색 */

	// 학교admin 권한//
	String subject; // 과목명
	String sbjNum; // 학수번호
	String CLP; // 신청학점//강의시간 3-2-2 parseInt(.index(0))//실습시간
	// int lectureTime;
	// int practiceTime;
	String openDepartment; // 개설학과

	// 학과admin 권한//

	String open = "0"; // 개설여부=> 학교admin이 과목을 생성하고 학과admin이 개설한다 0개설X 1개설O 일단 초기값은 개설이 안되어있으므로 0

	// 학교admin에서의 생성자
	public Subject(String subject, String sbjNum, String CLP, String openDepartment) {
		this.subject = subject;
		this.sbjNum = sbjNum;
		this.CLP = CLP;
		this.openDepartment = openDepartment;
	}

}

class OpenSubject { // 개설강의
	String subject; // 과목명
	String split; // 분반
	String season; // 개설학기(18-1, 16-2 ```)
	String room; // 강의실
	String pfsSubject; // 강의교수
	String maxStd; // 최대 인원 => 학과admin에서 30%이상 신청자가 없을 시 폐강되도록 한다.(open=true => false)
	String schedule; // 시간표 => 겹치는 시간 주의

	String sbjNum;
	String[] StuId;
	String ProId;
	String[] StuScore; // StuID mapping
    String finish = "0"; // 성적 확정 판단용
	public OpenSubject(String subject, String split, String season, String room, String pfsSubject, String maxStd, String schedule, String sbjNum, String ProId){
		this.subject=subject;
		this.split=split;
		this.season=season;
		this.room=room;
		this.pfsSubject=pfsSubject;
		this.maxStd=maxStd;
		this.schedule=schedule;
		this.sbjNum=sbjNum;
		this.ProId=ProId;
		StuId = new String[Integer.parseInt(maxStd)];
		StuScore = new String[Integer.parseInt(maxStd)];
	}
}
