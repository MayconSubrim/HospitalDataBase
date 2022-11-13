# O hostial é fudamental 🥼
## proposta:
Uma história para começar

Um pequeno hospital local busca desenvolver um novo sistema que atenda melhor às suas necessidades. Atualmente, parte da operação ainda se apoia em planilhas e arquivos antigos, mas espera-se que esses dados sejam transferidos para o novo sistema assim que ele estiver funcional. Neste momento, é necessário analisar com cuidado as necessidades desse cliente e sugerir uma estrutura de banco de dados adequada por meio de um Diagrama Entidade-Relacionamento.

# Solução:

![image](https://user-images.githubusercontent.com/110691979/197878245-4f834c13-838b-4d1f-abf7-c6bcb2876956.png)






# Part 2:

No hospital, as internações têm sido registradas por meio de formulários eletrônicos que gravam os dados em arquivos. 

Para cada internação, são anotadas a data de entrada, a data prevista de alta e a data efetiva de alta, além da descrição textual dos procedimentos a serem realizados. 

As internações precisam ser vinculadas a quartos, com a numeração e o tipo. 

Cada tipo de quarto tem sua descrição e o seu valor diário (a princípio, o hospital trabalha com apartamentos, quartos duplos e enfermaria).

Também é necessário controlar quais profissionais de enfermaria estarão responsáveis por acompanhar o paciente durante sua internação. Para cada enfermeiro(a), é necessário nome, CPF e registro no conselho de enfermagem (CRE).

A internação, obviamente, é vinculada a um paciente – que pode se internar mais de uma vez no hospital – e a um único médico responsável.



# Solução:



![Diagrama sem nome (1)](https://user-images.githubusercontent.com/110691979/200467739-2af7ed98-a8f6-4c88-bcff-67a41d549a3f.jpg)












Script no SQL Workbench:



create database hotel;

use hotel;

drop table if exists receita;
create table if not exists Receita(
	Id_Receita  int(11) auto_increment primary key,
    Medicamentos varchar(200), 
    instrucao varchar(200)
);

drop table if exists convenio;
create table if not exists convenio(
	ID_Convenio int(11) auto_increment primary key, 
	Nome varchar(35) not null,
    CNPJ int(14) not null,
    Tempo_carencia date not null
);

drop table if exists Paciente;
create table if not exists Paciente (
	ID_Paciente int (11) auto_increment primary key,
    nome varchar (100) not null,
    data_nascimento date not null,
    endereco varchar(20) not null,
    telefone varchar(20) not null,
    email varchar(30) not null,
    CPF int(11) not null,
    RG int(9) not null,
    ID_Convenio int(11) not null,
	FOREIGN KEY (ID_Convenio) REFERENCES convenio (ID_Convenio) ON DELETE CASCADE ON UPDATE CASCADE,
    Id_Receita int(11) not null,
	foreign key (Id_Receita) references Receita (Id_Receita) on update cascade on delete cascade
);

drop table if exists especialidades;
create table if not exists especialidades(
	ID_especialidades int(11) auto_increment primary key,	
	especialidade varchar(20) not null
);

drop table if exists Medico;
create table if not exists Medico(
	ID_Medic int(11) auto_increment primary key,
    Nome varchar(35) not null,
    CRM int(4) not null,
	tipo_medico varchar(50) not null,
    ID_especialidades int(11) not null,
	foreign key (ID_especialidades) references especialidades (ID_especialidades) on delete cascade on update cascade,
    Id_Receita int(11) not null,
	foreign key (Id_Receita) references Receita (Id_Receita) on update cascade on delete cascade

);

drop table if exists consulta;
create table if not exists consulta (
	ID_consulta int (11) AUTO_INCREMENT PRIMARY KEY,
	data_consulta date NOT NULL,
	hora_realizada time NOT NULL,
	ID_Medic INT(11) NOT NULL,
	FOREIGN KEY (ID_Medic) REFERENCES Medico (ID_Medic) ON DELETE CASCADE ON UPDATE CASCADE,
    ID_Paciente INT(11) NOT NULL,
    FOREIGN KEY (ID_Paciente) REFERENCES Paciente (ID_Paciente) ON DELETE CASCADE ON UPDATE CASCADE,
    valor_cobsulta varchar(100) not null,
    ID_Convenio INT(11) NOT NULL,
    FOREIGN KEY (ID_Convenio) REFERENCES convenio (ID_Convenio) ON DELETE CASCADE ON UPDATE CASCADE,
    ID_especialidadeS int(11) not null,
    FOREIGN KEY (ID_especialidades) REFERENCES especialidades (ID_especialidades) ON DELETE CASCADE ON UPDATE CASCADE
);

drop table if exists Enfemeira;
create table if not exists Enfemeira(
	ID_enferm int(11) auto_increment primary key,
    nome varchar(30) not null,
    CPF int(11) not null,
    CRE int(11) not null
);

drop table if exists Tipo_Quarto;
create table if not exists Tipo_Quarto(
	ID_TIPOQUARTO int(11) auto_increment primary key,
	valor_diaria int(100),
	descricao varchar(150)
);

drop table if exists Quarto;
create table if not exists Quarto(
	ID_Quarto int(11) auto_increment primary key,
	ID_TIPOQUARTO int(11) not null,
	foreign key (ID_TIPOQUARTO) references Tipo_Quarto(ID_TIPOQUARTO) on update cascade on delete cascade,
    Numero int(20) not null,
    tipo varchar(40) not null
);

drop table if exists Internação;
create table if not exists Internação(
	ID_Internacao int(11) auto_increment primary key,
    ID_enferm int(11) not null,
    foreign key (ID_enferm) references Enfemeira (ID_enferm) on update cascade on delete cascade,
    ID_Paciente int(11) not null,
    FOREIGN KEY (ID_Paciente) REFERENCES Paciente (ID_Paciente) ON DELETE CASCADE ON UPDATE CASCADE,
    ID_Quarto int(11) not null,
	foreign key (ID_Quarto) references Quarto (ID_Quarto) on update cascade on delete cascade,
    ID_Medic int(11) not null,
	FOREIGN KEY (ID_Medic) REFERENCES Medico (ID_Medic) ON DELETE CASCADE ON UPDATE CASCADE,
	data_entrada date not null,
    data_alt_prev date not null,
    data_alta date not null,
    procedimento varchar(25) not null
);



Partee 3:

Jogando nas regras que você criou: 
Crie scripts de povoamento das tabelas desenvolvidas na atividade anterior
Observe as seguintes atividades: 
Inclua ao menos dez médicos de 

Ao menos sete especialidades (considere a afirmação de que “entre as especialidades há pediatria, clínica geral, gastroenterologia e dermatologia”).

Inclua ao menos 15 pacientes.

Registre 20 consultas de diferentes pacientes e diferentes médicos (alguns pacientes realizam mais que uma consulta). As consultas devem ter ocorrido entre 01/01/2015 e 01/01/2022. Ao menos dez consultas devem ter receituário com dois ou mais medicamentos.

Inclua ao menos quatro convênios médicos, associe ao menos cinco pacientes e cinco consultas.

Criar entidade de relacionamento entre médico e especialidade. 

Criar Entidade de Relacionamento entre internação e enfermeiro. 

Arrumar a chave estrangeira do relacionamento entre convênio e médico.

Criar entidade entre internação e enfermeiro.

Colocar chaves estrangeira dentro da internação (Chaves Médico e Paciente).

Registre ao menos sete internações. Pelo menos dois pacientes devem ter se internado mais de uma vez. Ao menos três quartos devem ser cadastrados. As internações devem ter ocorrido entre 01/01/2015 e 01/01/2022.

Considerando que “a princípio o hospital trabalha com apartamentos, quartos duplos e enfermaria”, inclua ao menos esses três tipos com valores diferentes.

Inclua dados de dez profissionais de enfermaria. Associe cada internação a ao menos dois enfermeiros.

Os dados de tipo de quarto, convênio e especialidade são essenciais para a operação do sistema e, portanto, devem ser povoados assim que o sistema for instalado.


# Registrando Consultas:

![Consultas](https://user-images.githubusercontent.com/110691979/200942376-3d109011-144f-4175-985b-363f02806702.png)

![Consultas2](https://user-images.githubusercontent.com/110691979/200942403-91d329c7-de3a-4c0f-8dfd-132b908639e7.png)

# Registrando Convenio:

![Convenio](https://user-images.githubusercontent.com/110691979/200942458-4ed1acf6-b42f-4db2-9491-0faf76a436d1.png)

# Registrando medicos e seus tipos:

![Medicos (2)](https://user-images.githubusercontent.com/110691979/200942879-31bd96d2-425c-4c9c-b97e-ba82d55d3791.png)


![Medicos (2)](https://user-images.githubusercontent.com/110691979/200942957-5d84d977-e148-4958-b04f-fe28b97ea91b.png)


![Especialista](https://user-images.githubusercontent.com/110691979/200942913-fa60f080-380c-43c5-a523-28cdfbedf69e.png)


# Registrandon Pacientes:


![pacientes](https://user-images.githubusercontent.com/110691979/200943235-0aeddc1e-0feb-4639-9e6e-7ab3c47d55a8.png)

![Pacientes2](https://user-images.githubusercontent.com/110691979/200943262-4d1f3164-32f3-437a-978d-2b9f8488c1ed.png)



# Registrando Emfermeiras:

![Enfermeira](https://user-images.githubusercontent.com/110691979/200943506-e780c298-dbe6-4ec2-99a1-eb1029b869ad.png)


# Registrando Quartos e seus tipos:


![quartos](https://user-images.githubusercontent.com/110691979/200943577-c2d16143-6c43-4a6c-8f8d-fabda6c60ab4.png)

![Tipos de quartos](https://user-images.githubusercontent.com/110691979/200943603-576932e2-c8ba-4039-b5f1-67ff7a4df045.png)

# Registrando Internações:

![Internação](https://user-images.githubusercontent.com/110691979/200943807-0d19af4f-ff1d-46bd-858e-0c0abe6fc0e6.png)




# Part 4 :

Pensando no banco que já foi criado para o Projeto do Hospital, realize algumas alterações nas tabelas e nos dados usando comandos de atualização e exclusão:

Crie um script que adicione uma coluna “em_atividade” para os médicos, indicando se ele ainda está atuando no hospital ou não. 

Crie um script para atualizar ao menos dois médicos como inativos e os demais em atividade.

# solução:

![image](https://user-images.githubusercontent.com/110691979/201248093-befb5fc1-a7b6-4436-a8f4-9ddc5d910c7b.png)

![image](https://user-images.githubusercontent.com/110691979/201248164-99e1d8fb-c745-45b0-b9eb-7ef414270eb2.png)

![image](https://user-images.githubusercontent.com/110691979/201248269-a32a6701-2be6-4d9b-81e9-b6c9c9203a2d.png)

![image](https://user-images.githubusercontent.com/110691979/201248292-5de985f3-f7c1-4cfc-b887-df15980cc7d6.png)





Part 4:




Solução:


-- R1
select *, avg(valor_cobsulta)from consulta group by ID_Convenio having year(data_consulta) = '2020';

-- R2
select * from internação where data_alta > data_alt_prev;

-- R3
select 
	c.*,
    p.*,
    r.* 
from consulta c inner join paciente p 
on c.ID_Paciente  = p.ID_Paciente inner join receita r 
    on p.Id_Receita = r.Id_Receita  limit 1;

-- R4    
select *, Max(valor_cobsulta), Min(valor_cobsulta) from consulta group by ID_Convenio is null;

-- R5
select 
	 internação.*,
	 quarto.*, 
	 datediff(data_alta, data_entrada) dias_em_uso,
	 tipo_quarto.valor_diaria, 
	 datediff(data_alta, data_entrada) * tipo_quarto.valor_diaria valor_total 
 from internação
 inner join quarto on internação.ID_Quarto = quarto.ID_Quarto 
 inner join tipo_quarto on quarto.ID_TIPOQUARTO = tipo_quarto.ID_TIPOQUARTO 
 group by ID_Internacao;
 
-- R6 
Select i.data_entrada, i.procedimento, q.Numero from internação i 
inner join quarto q on q.ID_Quarto = i.ID_Quarto where ID_TIPOQUARTO = 1;

-- R7
select p.nome, c.data_consulta, e.especialidade from consulta c
 inner join paciente p on p.ID_Paciente = c.ID_Paciente 
 inner join especialidades e on e.ID_especialidadeS = c.ID_especialidadeS
 where c.ID_especialidadeS <> 1 and  
 YEAR(c.data_consulta) - YEAR(p.data_nascimento) < 19 and 
 YEAR(c.data_consulta) - YEAR(p.data_nascimento) > 0
 order by c.data_consulta;

-- R8
select p.nome, m.Nome, i.data_entrada, i.procedimento, q.ID_TIPOQUARTO from internação i 
inner join medico m on m.ID_Medic = i.ID_Medic
inner join paciente p on p.ID_Paciente = i.ID_Paciente
inner join quarto q on q.ID_Quarto = i.ID_Quarto
where q.ID_TIPOQUARTO = 3
and m.ID_especialidades = 3;

-- R9
select m.Nome, m.CRM, count(c.ID_Medic) as 'Quantidade de consulta' from medico m 
inner join consulta c
on m.ID_Medic = c.ID_Medic
group by c.ID_Medic;

-- R10
select * from medico where nome like "%Gabriel%";

-- R11
select e.nome, e.CRE, count(i.ID_enferm) Numero from enfemeira e
inner join internação i on e.ID_enferm = i.ID_enferm 
group by e.ID_enferm having Numero > 1;

    
    
    
    
    










