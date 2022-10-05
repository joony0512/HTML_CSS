//전체 평균보다 큰 평균(잡코드별)만 뽑기
proc sql;
select JobCode, avg(Salary) as MeanSalary
from a.payrollmaster
group by Jobcode
having avg(Salary) > (select avg(Salary)  from a.payrollmaster ) ;
quit;

//서브쿼리를 조건으로 쿼리 실행
proc sql;
select LastName, FirstName, City, State
from a.staffmaster
where EmpID in (select EmpID
					from a.payrollmaster
					where month(DateOfBirth)=2);
quit;

//any 사용법
proc sql;
select LastName, FirstName, City, State
from a.staffmaster
where EmpID = any (select EmpID
					from a.payrollmaster
					where month(DateOfBirth)=2);
quit;

// 서브쿼리 안의 내용 중 하나보다라도 작으면 됨 = max
proc sql;
select FFID, Name, Milestraveled
from a.frequentflyers
where membertype='GOLD'
and milestraveled < any
						(select milestraveled
						from a.frequentflyers
						where membertype in ('BRONZE', 'SILVER'));
quit;

//all 키워드 사용 any와 반대 : 서브쿼리 안 모든것보다 작음 = min
proc sql;
select FFID, Name, Milestraveled
from a.frequentflyers
where membertype='GOLD'
and milestraveled < all
						(select milestraveled
						from a.frequentflyers
						where membertype in ('BRONZE', 'SILVER'));
quit;
