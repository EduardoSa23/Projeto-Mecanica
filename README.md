# Projeto-Mecanica

![Projeto mecanica](https://user-images.githubusercontent.com/111080797/192118789-aade54b9-dff9-429d-ad47-43540fe35f92.png)


-- drop database OficinaMecanica;
create database OficinaMecanica;
use OficinaMecanica;

-- criar tabela cliente
-- drop table clientes;


create table clientes (
	idClientes int auto_increment primary key,
    Pnome Varchar(20),
    Mnome varchar(3),
    Snome varchar(20),
    CPF char(11) not null,
    Endereco varchar(250),
    Contato varchar(11),
    constraint unique_cpf_clientes unique (CPF)
);


create table mecanicos(
	idMecanicos int auto_increment primary key,
    NomeMeca varchar(45) not null,
    Codigo int not null,
    MecaEndereco varchar(250) not null
);

create table especialidades(
	idEspecialidades int auto_increment,
    idMecMecanicos int,
    Parteeletrica char default null,
    Motor char default null,
    Cambio char default null,
    Injecaoeletronica char default null,
    Suspensao char default null,
    Motordearranque char,
    primary key (idEspecialidades, idMecMecanicos),
    constraint especialidades_mecanicos foreign key(idMecMecanicos) references mecanicos(idMecanicos)
);

alter table especialidades auto_increment=1;

-- drop table especialidadesmecanicos;
-- create table especialidadesmecanicos (
-- idEsEspecialidades int,
-- idEsMecanicos int,
-- primary key (idEsEspecialidades, idEsMecanicos),
-- constraint id_Especialidade_mecanicos foreign key (idEsMecanicos) references mecanicos(idMecanicos),
-- constraint id_epecialidade_especialidade foreign key (idEsEspecialidades) references especialidades(idEspecialidades)
-- );

create table execucao(
	idExecucao int auto_increment,
    idExClientes int,
    idExMecanicos int,
    Autorizacao enum('Autorizado','Não autorizado') default null,
    primary key (idExecucao, idExClientes, idExMecanicos),
    constraint idExecucao_Clientes foreign key (idExClientes) references clientes (idClientes),
    constraint idExecucao_Mecanicos foreign key (idExMecanicos) references mecanicos (idMecanicos)
);

alter table execucao auto_increment=1;


create table orcamento(
	idOrcamento int auto_increment,
    idOrClientes int,
    Revisao char default null,
    Conserto char,
    primary key (idOrcamento, idOrClientes),
    constraint idOrcamento_clientes foreign key (idOrClientes) references clientes(idClientes)
);

alter table orcamento auto_increment=1;

create table mecanicoorcamento(
	idMoMecanicos int,
    idMoOrcamento int,
    primary key (idMoMecanicos, idMoOrcamento),
    constraint idMecanicoorcamento_Mecanicos foreign key (idMoMecanicos) references mecanicos(idMecanicos),
	constraint idMecanicoorcamento_Orcamento foreign key (idMoOrcamento) references orcamento(idOrcamento)
);

create table carro(
	idCarro int auto_increment,
    idCarClientes int,
    NomeCar varchar(15) not null,
    Modelo varchar(15) not null,
    Marca varchar(15) not null,
    Cor varchar(15) not null,
    Ano int not null,
    Placa char(7),
	primary key (idCarro, idCarClientes),
    constraint idCarClientes_Clientes foreign key (idCarClientes) references clientes(idClientes)
);

alter table carro auto_increment=1;

create table ordemservico(
	idOrdemServico int auto_increment primary key,
    Numeros varchar(10)not null,
    StatusOs enum('Aguardando autorização','Em conserto','Em revisão','Pronto') default 'Aguardando autorização',
    DataEmissao date,
    DataEntrega date,
    Valor decimal(5,2),
    constraint unique_Numeros_clientes unique (Numeros)
);

alter table ordemservico auto_increment=1;

create table orcamentoOs(
	idOrOsOrcamento int,
    idOrOsOrderservico int,
    primary key (idOrOsOrcamento,idOrOsOrderservico),
    constraint id_orcamentoOs_orcamento foreign key(idOrOsOrcamento) references orcamento(idOrcamento),
    constraint id_orcamentoOs_ordemservico foreign key(idOrOsOrderservico) references ordemservico(idOrdemServico)
);

create table valormaodeobra(
	idValormaodeobra int auto_increment,
    idMaoCarro int,
    Parteeletrica decimal(5,2),
    Motor decimal(5,2),
    Cambio decimal(5,2),
    Injecaoeletronica decimal(5,2),
    Suspensao decimal(5,2),
    Motordearranque decimal(5,2),
    primary key (idValormaodeobra,idMaoCarro),
    constraint idvalormaodeobra_carro foreign key(idMaoCarro) references carro(idCarro)
);
alter table ordemservico auto_increment=1;

create table valormaodeobraOs (
	idMaoOsValor int,
    idMaoOsOrdemservico int,
    primary key (idMaoOsValor,idMaoOsOrdemservico),
    constraint idvalormaodeobraOs_Valor foreign key(idMaoOsValor) references valormaodeobra(idValormaodeobra),
    constraint idvalormaodeobraOs_Ordemservico foreign key (idMaoOsOrdemservico) references ordemservico(idOrdemservico)
);

create table trocapecas(
	idTrocapecas int auto_increment primary key,
    Nome varchar(20),
    Quantidade int,
    Valor decimal(5,2)
);

alter table trocapecas auto_increment=1;

create table trocapecasOs(
	idTrocaOsPecas int,
    idTrocaOsOrdemservico int,
    primary key (idTrocaOsPecas, idTrocaOsOrdemservico),
    constraint idTrocapecasOs_Pecas foreign key(idTrocaOsPecas) references trocapecas(idTrocapecas),
    constraint idTrocapecasOs_Ordemservico foreign key(idTrocaOsOrdemservico) references ordemservico(idOrdemservico)
);


_______________________________________________________________________________________________________________________________________________________________

use OficinaMecanica;

-- show tables;
-- desc clientes;


insert into clientes (Pnome, Mnome, Snome, CPF, Endereco, Contato)
	values('Eduardo','A','Sá','47362746352','rua sobral 35, Calmon Viana, Poá, SP', '11964736254'),
		  ('Carlos','S','Santos','57483956439','Av do estado 3904, Pari, São Paulo, SP', '11965748362'),
          ('Denis','B','Oliveira','89058903625','Rua Charles 907, Centro, Mogi das Cruzes, SP', '11965740982'),
          ('Camila','G','Rubens','56473859067','Alameda Cointra 64, Costa, Suzano, SP', '11964537385'),
          ('Palmira','T','Aparecida','57849038002','Av 9 de julho 1075, Centro, Poá, SP', '11905647380'),
          ('Dorival','O','Silva','56473820984','Rua amazonas 25, Virgina, Itaquaquecetuba, SP', '11965840382');
    
insert into mecanicos(NomeMeca, Codigo, MecaEndereco)
	values('Civaldo', 1, 'Rua silva 908, Largo, Suzano'),
		  ('Elivelton', 2, 'Av junqueira 1098, Centro, Poá'),
          ('Tonhão', 3, 'Rua edgar 15, vila mara, Suzano'),
          ('Limeira', 4, 'Al Soares 67, Eureka, Poá'),
          ('Paraiba', 5, 'Rua salles 674, centro, Itaquaquecetuba');


insert into especialidades(idMecMecanicos, Parteeletrica, Motor, Cambio, Injecaoeletronica, Suspensao, Motordearranque)
	values(1,'S','N','N','S','S','S'),
		  (2,'N',null,'S','N','S','S'),
          (3,'N','S','N','N',null,'S'),
          (4,null,'S',null,'N','N','N'),
          (5,'S','N','N','N',null,'N');

insert into execucao(idExClientes, idExMecanicos, Autorizacao)
	values(1,2,'Autorizado'),
		  (4,5,'Não Autorizado'),
          (2,2,'Autorizado'),
          (3,1,'Autorizado'),
          (6,4,'Não autorizado'),
          (5,3,'Autorizado');
          
insert into orcamento(idOrClientes, Revisao, Conserto)
	values(2,'S','N'),
		  (1,'N','S'),
          (5,'N','S'),
          (6,'S','N'),
          (3,'N','S'),
          (4,'S','N');
          
insert into mecanicoorcamento(idMoMecanicos, idMoOrcamento)
	values(1,3),
		  (3,2),
          (4,1),
          (2,6),
          (5,4),
          (4,5);
          
insert into carro(idCarClientes, NomeCar, Modelo, Marca, Cor, Ano, Placa)
	values(5,'Tigo','SUV','Caoa Cherry','Vermelho','2018','CPD4G89'),
		  (3,'Fiesta','Sedan','Ford','Preto','2007','CGY8H54'),
          (6,'Palio','Hatch','Fiat','Prata','2003','PGH2J59'),
          (2,'Gol','Hatch','Wolksvagen','Prata','2010','JCB8N24'),
          (1,'Tucson','SUV','Hyundai','Preto','2012','LHP6M09'),
          (2,'Etios','Hatch','Toyota','Azul','2015','HDI9S08'),
          (4,'Creta','SUV','Chevrolet','Vermelho','2020','JHI6L93'),
          (1,'Astra','Sedan','Chevrolet','Preto','2010','HDG2V57');
          
insert into ordemservico(Numeros, StatusOs, DataEmissao, DataEntrega)
	values(5674,'Em conserto','2022-08-23','2022-08-26'),
		  (354,'Aguardando Autorização','2022-09-20','2022-10-10'),
          (276,'Pronto','2022-08-23','2022-08-26'),
          (4783,'Em revisão','2022-09-06','2022-09-07'),
          (2564,'Em conserto','2022-09-05','2022-09-03'),
          (3948,'Em conserto','2022-09-10','2022-09-20'),
          (5847,'Pronto','2021-02-23','2021-03-15'),
          (4758,'Aguardando Autorização','2022-09-12','2022-09-26');
          
insert into orcamentoOs(idOrOsOrcamento,idOrOsOrderservico)
	values(1,4),
		  (2,3),
          (4,1),
          (6,6),
          (5,2),
          (3,5);
          
insert into valormaodeobra (idMaoCarro, Parteeletrica, Motor, Cambio, Injecaoeletronica, Suspensao, Motordearranque)
	values(1,0.0,0.0,200.00,0.0,0.0,0.0),
		  (5,250.00,0.0,0.0,0.0,0.0,0.0),
          (3,0.0,900.00,0.0,0.0,0.0,80.00),
          (6,0.0,0.0,0.0,0.0,450.0,0.0),
          (2,0.0,800.00,600.00,0.0,0.0,0.0),
          (4,0.0,230,0.0,0.0,0.0,130.00);
          
insert into valormaodeobraOs(idMaoOsValor, idMaoOsOrdemservico)
	values(2,5),
		  (4,6),
          (3,2),
          (6,4),
          (5,1),
          (1,3);
          
insert into trocapecas(Nome, Quantidade, Valor)
	values('Pistão',4,950.00),
		  ('Coxinho',2,200.00),
          ('Cilindro',1,450.00);
          
insert into trocapecasOs(idTrocaOsPecas, idTrocaOsOrdemservico)
	values(1,3),
		  (2,1),
          (3,2);

-- Quantos carros estão em conserto
select count(*) as Carros_em_Conserto from ordemservico where statusOs = 'Em conserto';

-- Quantos carros passaram ou estão na oficina
select count(*) from clientes c, carro o where c.idClientes = o.idCarClientes;

-- Valores ordenados 
select * from ordemservico order by DataEmissao;
          

