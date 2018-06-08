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
	// static int numOfPerson=0;

	public Person(String personalNum, String phoneNumber, String email, String name, String department) {
		this.ID = personalNum;
		this.phoneNumber = phoneNumber;
		this.email = email;
		this.name = name;
		this.department = department;
		this.PW = personalNum.substring(personalNum.length()-4);
	    this.joinDay = personalNum.substring(0, 4) + ".3.2";
	}
	//패스워드 입력 생성자 ,   파일 로딩시 or 메뉴실행중 학생 추가시
	public Person(String personalNum, String phoneNumber, String email, String name, String department, String pw) {
		this.ID = personalNum;
		this.phoneNumber = phoneNumber;
		this.email = email;
		this.name = name;
		this.department = department;
		this.PW = pw;
		java.util.Random r1 = new java.util.Random();
	    this.joinDay = personalNum.substring(0, 4) + "."+(r1.nextInt(12)+1)+"."+(r1.nextInt(28)+1);
	}
    
    public void print() {
		System.out.print("전화번호: " + phoneNumber + " 이메일:" + email + " 이름:" + name + " 학과:" + department);
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
		this.joinDay = personalNum.substring(0, 4)+".3.2";
		stdNum++;
	}
	// 파일 로드시 용도
	public Student(String personalNum, String phoneNumber, String email, String name, String department, 
			String tuition, String pw) {
		super(personalNum, phoneNumber, email, name, department,pw);
		this.tuition = tuition;
		this.joinDay = personalNum.substring(0, 4)+".3.2";
		stdNum++;
	}
    
    public void print() {
		System.out.print("학번:" + ID);
		super.print();
		System.out.println("등록금:" + tuition + " 입학일:" + joinDay);
	}

}

class Scholarship {// 해당 학기의 등록금을 장학금이 넘지 못하도록 구현 필요
	static int scoNum = 0;
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
        scoNum++;
    }
    
    public void print() {
		System.out.println("학번:" + ID + " 장학금명:" + name + " 금액:" + money + " 수혜학기:" + semester);
	}
}
class Admin extends Person {

	String salary;
	String officeNumber;
    	public Admin(String personalNum, String phoneNumber, String officeNumber, String email, String name, String department, String salary
			) {
		super(personalNum, phoneNumber, email, name, department);
		this.salary = salary;
		this.officeNumber = officeNumber;

	}
    
	public Admin(String personalNum, String phoneNumber, String officeNumber, String email, String name, String department, String salary,
			String pw) {
		super(personalNum, phoneNumber, email, name, department,pw);
		this.salary = salary;
		this.officeNumber = officeNumber;

	}
    
     public void Print() {
		System.out.print("사번:" + ID);
		super.print();
		System.out.println(" 월급:" + salary + " 사무실번호:" + officeNumber);
	}

}

class Professor extends Admin {
	//String subject; //맡은 과목 여러개이므로 열린과목 클래스의 proId값과 매칭해서 검사
	String position;

	// 비번이 없는경우//
	public Professor(String personalNum, String phoneNumber, String officeNumber, String email, String name,
			String department, String salary, String position) {
		super(personalNum, phoneNumber,officeNumber, email, name, department,salary);
		this.position = position;
		pNum++;
	}
	// 비번이 있는경우//
	public Professor(String personalNum, String phoneNumber, String officeNumber, String email, String name,
			String department, String salary, String position, String pw) {
		super(personalNum, phoneNumber,officeNumber, email, name, department,salary,pw);
		this.position = position;
		pNum++;
	}
    
    public void print() {
		super.Print();
		System.out.println(" 직급:" + position);
	}
}



class Staff extends Admin {
	String position;

	public Staff(String personalNum, String phoneNumber, String officeNumber, String email, String name, String department, String salary,
			String position,String joinday, String pw) {
		super(personalNum, phoneNumber,officeNumber, email, name, department, salary, pw);
		this.position = position;
		this.joinDay = joinday;
		sNum++;

	}
    
    public void print() {
		super.Print();
		System.out.println(" 직급:" + position);
	}
    
}

class Assistant extends Admin {

	public Assistant(String personalNum, String phoneNumber, String officeNumber, String email, String name, String department,
			String salary, String pw) {
		super(personalNum, phoneNumber,officeNumber, email, name, department, salary, pw);
		aNum++;
	}
    
    public void print() {
		super.Print();
	}
}


class Department {
    static int dnum = 0;
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
        dnum++;
	}
	
    public void print() {
		System.out.println("학과명:" + department + " 학과주소:" + address + " 학과 전화번호:" + Number + " 학과 조교이름:" + assistantName
				+ " 학과장 이름:" + capName + " 학과 이메일" + Email + " 학과 웹사이트:" + Web);
		System.out.println();
	}

	// 학과검색//
	/*
	 * public String getDepartment() { return this.department; }
	 */

}

class Subject {
	/* 교과목 검색은 과목명, 학수번호, 개설학과로 검색 */
    static int snum = 0;
	// 학교admin 권한//
	String subject; // 과목명
	String sbjNum; // 학수번호
	String CLP; // 신청학점//강의시간 3-2-2 parseInt(.index(0))//실습시간
	// int lectureTime;
	// int practiceTime;
	String openDepartment; // 개설학과

	// 학과admin 권한//

	// 학교admin에서의 생성자
	public Subject(String subject, String sbjNum, String CLP, String openDepartment) {
		this.subject = subject;
		this.sbjNum = sbjNum;
		this.CLP = CLP;
		this.openDepartment = openDepartment;
        sNum++;
	}
    
    public void print() {
		System.out.println("과목명:" + subject + " 학수번호:" + sbjNum + " 신청학점-강의시간-실습시간:" + CLP + " 개설학과:" + openDepartment);
	}

}

class OpenSubject { // 개설강의
	String subject; // 과목명
	String split; // 분반
	String season; // 개설학기(2018-1, 2016-2 ```)
	String room; // 강의실
	String pfsSubject; // 강의교수
	String maxStd; // 최대 인원 => 학과admin에서 30%이상 신청자가 없을 시 폐강되도록 한다.(open=true => false)
	String schedule; // 시간표 => 겹치는 시간 주의
	String CLP;
	String sbjNum;
	
	String[] StuId;
	String ProId;
	String[] StuScore; // StuID mapping 0~100 점수 넣기
    String finish = "0"; // 성적 확정 판단용 0: 진행 1: 종료 2:폐강
	static int Onum=0;
	public OpenSubject(String subject, String split, String season, String room, String pfsSubject, String maxStd, String schedule, String sbjNum, String ProId, String CLP){
		this.subject=subject;
		this.split=split;
		this.season=season;
		this.room=room;
		this.pfsSubject=pfsSubject;
		this.maxStd=maxStd;
		this.schedule=schedule;
		this.sbjNum=sbjNum;
		this.ProId=ProId;
		this.CLP=CLP;
		StuId = new String[Integer.parseInt(maxStd)];
		StuScore = new String[Integer.parseInt(maxStd)];
		if(!(season.equals("2018-2")))//현재 학기 과목이 아닐경우
			finish="1"; // 종강과목
		Onum++;
	}
	public void print(){//수강 신청 or 수강 중 확인용
		System.out.print("학수번호:"+sbjNum+" 과목명:"+ subject);
		System.out.println(" 분반:"+0+split+" 강의실:"+room+" 강의 교수:"+pfsSubject+" 시간표:"+schedule);
	}
	
	public void printAll(int n){//이미 이수한 과목 출력
		System.out.print("년도:"+season.substring(0, 4)+" 학기:"+season.substring(6, 7)+" 학수번호:"+sbjNum+" 과목명:"+subject+" 학점"+CLP.substring(0, 1));
		if((Integer.parseInt(StuScore[n]))>96)
			System.out.println("등급:A+ 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>92)
			System.out.println("등급:A0 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>89)
			System.out.println("등급:A- 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>86)
			System.out.println("등급:B+ 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>82)
			System.out.println("등급:B0 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>79)
			System.out.println("등급:B- 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>76)
			System.out.println("등급:C+ 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>72)
			System.out.println("등급:C0 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>69)
			System.out.println("등급:C- 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>66)
			System.out.println("등급:D+ 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>62)
			System.out.println("등급:D0 평점 평균:"+StuScore[n]);
		else if((Integer.parseInt(StuScore[n]))>59)
			System.out.println("등급:D- 평점 평균:"+StuScore[n]);
		else
			System.out.println("등급:F 평점 평균:"+StuScore[n]);
	}
	
}
