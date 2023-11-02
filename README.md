# Medico_Helpers

## Procedures
```
create or replace procedure quantity (pro varchar2, medShop int) as
	cursor c_quantity is select sale.quantity as qty from sale where sale.med_shop_id = medShop and sale.code in ( select equipment.code from equipment where equipment.product_name = pro);

	r_quantity c_quantity%rowtype;
begin 
	for r_quantity in c_quantity loop
		dbms_output.put_line ('Quantity present | '||r_quantity.qty);
	end loop;
end;

declare
begin
quantity('Crocin',1);
end;
/
```

```
create or replace procedure sum_amount (medShopId int) as
	cursor c_sum_amount is select sum(amount) as sum from bill where bill.med_shop_id = medShopId;
	r_sum_amount c_sum_amount%rowtype;

begin 
	for r_sum_amount in c_sum_amount loop
		dbms_output.put_line('Sum of amount collected from selling : '||r_sum_amount.sum);
	end loop;
end;

declare
begin
sum_amount(1);
end;
/
```

```
create or replace procedure patient_names (doctor_id int) as
	cursor c_patient_names is select customer.name, customer.gender, customer.phone from customer where customer.pat_id in (select prescribe.pat_id from prescribe where prescribe.doc_id = doctor_id);

	r_patient_names c_patient_names%rowtype;

begin 
	for r_patient_names in c_patient_names loop
		dbms_output.put_line(r_patient_names.name||' | '||r_patient_names.gender||' | '||r_patient_names.phone);
	end loop;
end;

declare
begin
patient_names(105);
end;
/
```

```
create or replace procedure employee_details (medShopId int) as
	cursor c_employee_details is select employee.name,employee.mobile_no from employee where employee.med_shop_id in (select med_shop.med_shop_id from med_shop where med_shop.med_shop_id = medShopId);
	r_employee_details c_employee_details%rowtype;

begin 
	for r_employee_details in c_employee_details loop
		dbms_output.put_line(r_employee_details.name||' '||r_employee_details.mobile_no);
	end loop;
end;

declare
begin
employee_details(1);
end;
/
```

```
create or replace procedure med_details (ptName varchar2, med int) as
	cursor c_med_details is select product,date_of_sale from bill where pat_name = ptName and med_shop_id = med;
	r_med_details c_med_details%rowtype;

begin 
	for r_med_details in c_med_details loop
		dbms_output.put_line(r_med_details.product||' '||r_med_details.date_of_sale);
	end loop;
end;

declare
begin
med_details('Irfan',1);
end;
/
```

```
create or replace procedure hos_details (pharName varchar2) as
	cursor c_hos_details is select hospital.name,hospital.city,phone from hospital where hospital.med_shop_id in (select med_shop.med_shop_id from med_shop where name =pharName);
	r_hos_details c_hos_details%rowtype;

begin 
	for r_hos_details in c_hos_details loop
		dbms_output.put_line(r_hos_details.name||' '||r_hos_details.city||' | '||r_hos_details);
	end loop;
end;
/

declare
begin
hos_details('Mahavir');
end;
/
```

```
create or replace procedure loyal (pharID int) as
	cursor c_loyal is select pat_name, date_of_sale, product from bill where med_shop_id = pharID;
	r_loyal c_loyal%rowtype;
begin 
	for r_loyal in c_loyal loop
		dbms_output.put_line(r_loyal.pat_name||' | '||r_loyal.date_of_sale||' | '||r_loyal.product);
	end loop;
end;
/

declare
begin
loyal(1);
end;
/
```

```
create or replace procedure spec (spec varchar2) as
	cursor c_spec is select doctor.doc_name, doctor.mobile from doctor where speciality = spec;
	r_spec c_spec%rowtype;
begin 
	for r_spec in c_spec loop
	dbms_output.put_line(r_spec.doc_name||' | '||r_spec.mobile);
	end loop;
end;
/

declare
begin
spec('Gynecologist');
end;
/
```

```
create or replace procedure doctor_names (patient_id int) as
	cursor c_doctor_names is select doctor.doc_name, SPECIALITY, doctor.mobile from doctor where doctor.doc_id in (select prescribe.doc_id from prescribe where prescribe.pat_id = patient_id);

	r_doctor_names c_doctor_names%rowtype;

begin 
	for r_doctor_names in c_doctor_names loop
		dbms_output.put_line(r_doctor_names.doc_name||' | '||r_doctor_names.SPECIALITY||' | '||r_doctor_names.mobile);
	end loop;
end;

declare
begin
doctor_names(205);
end;
/
```

```
create or replace procedure expiry(phID int) as
	cursor c_exp is select product_name,exp_date from equipment where sysdate > exp_date and equipment.code in (select sale.code from sale where med_shop_id = phID);
	r_exp c_exp%rowtype;
begin 
	for r_exp in c_exp loop
	dbms_output.put_line(r_exp.product_name||' | '||r_exp.exp_date);
	end loop;
end;
/

declare
begin
expiry(1);
end;
/
```

```
create or replace procedure chk_med (name varchar2, id int) as
	cursor c_chk_med is select quantity from sale where sale.med_shop_id = id and sale.code in (select equipment.code from equipment where equipment.product_name = name);
	r_chk_med c_chk_med%rowtype;
begin 
	for r_chk_med in c_chk_med loop
	dbms_output.put_line('Quantity : '||r_chk_med.quantity);
	end loop;
end;
/

declare
begin
chk_med('Crocin',1);
end;
/
```

```
create or replace procedure sup (phiD int) as
	cursor c_sup is select supplier.name,supplier.mobile_no from supplier where supplier.med_shop_id = phiD;
	r_sup c_sup%rowtype;
begin 
	for r_sup in c_sup loop
	dbms_output.put_line(r_sup.name||' | '||r_sup.mobile_no);
	end loop;
end;

declare
begin
sup(1);
end;
/
```

```
create or replace procedure stock(phID int) as
	cursor c_stock is select product_name from equipment where equipment.code in ( select sale.code from sale where sale.med_shop_id = phid) order by product_name;
	cursor c_qty (nm equipment.product_name%type) is select quantity from sale where sale.code in (select equipment.code from equipment where product_name = nm) and sale.med_shop_id = phID;
	r_stock c_stock%rowtype;
	r_qty c_qty%rowtype;
begin 
	for r_stock in c_stock loop
		for r_qty in c_qty(r_stock.product_name) loop
			dbms_output.put_line(r_stock.product_name||' | ' ||r_qty.quantity);
		end loop;
	end loop;
end;
/

declare
begin
stock(1);
end;
/
```

## Triggers
```
create or replace trigger age_chk
before insert or update on doctor
for each row
begin
        if(:new.age<30 and :new.age>75) then
               raise_application_error(-0021,'Age of doctor must be between 30 and 75');
        end if;
end;
/
```

```
create or replace trigger inquiry before insert on sale
for each row
declare
 cursor c_inquiry(cid sale.med_shop_id%type) is select invoice.code as cd from invoice where invoice.med_shop_id = cid;
 r_inquiry c_inquiry%rowtype;
 chk int;
begin 
	if(:new.med_shop_id = 1) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if; 

	if(:new.med_shop_id = 2) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

	if(:new.med_shop_id = 3) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

	if(:new.med_shop_id = 3) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

	if(:new.med_shop_id = 4) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

	if(:new.med_shop_id = 5) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

	if(:new.med_shop_id = 6) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

	if(:new.med_shop_id = 7) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

	if(:new.med_shop_id = 8) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

	if(:new.med_shop_id = 9) then
	 for r_inquiry in c_inquiry(:new.med_shop_id) loop
	 	if(:new.code = r_inquiry.cd) then
	 		chk := 1;
	 	end if;
	 end loop;
	 if (chk = 0) then 
	  raise_application_error(-9001,'Cannnot sell those item which are not imported');
	 end if;
	end if;

end;
/
```

```
create or replace trigger med_shop_del before delete on med_shop
for each row 
begin
	delete from employee where :old.med_shop_id = employee.med_shop_id;
	delete from supplier where :old.med_shop_id = supplier.med_shop_id;
	delete from sale where :old.med_shop_id = sale.med_shop_id;
	delete from invoice where :old.med_shop_id = invoice.med_shop_id;
	update hospital set hospital.med_shop_id = null where hospital.med_shop_id = :old.med_shop_id;
end;
/
```

```
create or replace trigger qty after insert on bill
for each row 
begin
	update sale set quantity = (quantity-1) where sale.med_shop_id = :new.med_shop_id and sale.code in (select equipment.code from equipment where equipment.product_name = :new.product);
end;
```

```
create or replace trigger tr_emp_shift before delete on employee
for each row
begin
delete from works where mobile_no=:old.mobile_no;
end;
/

```

