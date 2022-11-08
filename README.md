# O hostial é fudamental 🥼
## proposta:
Uma história para começar

Um pequeno hospital local busca desenvolver um novo sistema que atenda melhor às suas necessidades. Atualmente, parte da operação ainda se apoia em planilhas e arquivos antigos, mas espera-se que esses dados sejam transferidos para o novo sistema assim que ele estiver funcional. Neste momento, é necessário analisar com cuidado as necessidades desse cliente e sugerir uma estrutura de banco de dados adequada por meio de um Diagrama Entidade-Relacionamento.

# Solução:

![image](https://user-images.githubusercontent.com/110691979/197878245-4f834c13-838b-4d1f-abf7-c6bcb2876956.png)






Part 2:

No hospital, as internações têm sido registradas por meio de formulários eletrônicos que gravam os dados em arquivos. 

Para cada internação, são anotadas a data de entrada, a data prevista de alta e a data efetiva de alta, além da descrição textual dos procedimentos a serem realizados. 

As internações precisam ser vinculadas a quartos, com a numeração e o tipo. 

Cada tipo de quarto tem sua descrição e o seu valor diário (a princípio, o hospital trabalha com apartamentos, quartos duplos e enfermaria).

Também é necessário controlar quais profissionais de enfermaria estarão responsáveis por acompanhar o paciente durante sua internação. Para cada enfermeiro(a), é necessário nome, CPF e registro no conselho de enfermagem (CRE).

A internação, obviamente, é vinculada a um paciente – que pode se internar mais de uma vez no hospital – e a um único médico responsável.

















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












