# Banco-De-Dados-DIO
Banco de Dados desenvolvido para estudo de SQL

-- criação do bd para o cenario de E-commerce

create database ecommerce;
use ecommerce;

-- criar tabela cliente

create table client(
	idClient int AUTO_INCREMENT PRIMARY KEY,
    Fname varchar(10),
    Minit char(3),
    Lname varchar(20),
    CPF char(11) not null,
    Address varchar(30),
    CONSTRAINT unique_cpf_client unique(CPF)
);

-- criar tabela produto
-- Size = dimesão do produto

create table product(
	idProduct int AUTO_INCREMENT PRIMARY KEY,
    Pname varchar(10) not null,
    classification_kids boolean DEFAULT false,
    category enum ('Eletrônico', 'Vestimenta', 'Brinquedos', 'Alimentos') not null,
    avaliacao float default 0,
    size varchar(10)
);

-- criar tabela pagamentos
-- para ser continuado no desafio: termine de implementar a tabela e crie a conexão com as tabelas necessárias 

create table payments(
	idClient int,
    id_payments int,
    typePayment enum('Boleto', 'Cartão', 'Dois Cartões'),
    limitAvaliable float,
    primary key(idClient, id_payments)
);

-- criar tabela pedido

create table orders(
	idOrder int AUTO_INCREMENT PRIMARY KEY,
    idOrderClient int,
    orderStatus enum('Cancelado', 'Confirmado', 'Em processamento') not null,
    orderDescription varchar(255),
    sendValue float DEFAULT 10,
    paymentCash boolean default false,
    constraint fk_orders_client foreign key (idOrderClient) references client(idClient)   
);

-- criar tabela estoque

create table productStorage(
	idProductStorage int AUTO_INCREMENT PRIMARY KEY,
    storageLocation varchar(255),
    quantity float DEFAULT 0,
    paymentCash boolean default false
);

-- criar tabela forncedor

create table supplier(
	idSupplier int AUTO_INCREMENT PRIMARY KEY,
    socialName varchar(255) not null,
    CNPJ char(15) not null,
    contact char(11) not null,
    constraint unique_supplier unique (CNPJ)
);

-- criar tabela vendedor
create table seller(
    idSeller int auto_increment primary key,
    SocialName varchar(255) not null,
    AbsName varchar(255),
    CNPJ char(15),
    CPF char(9),
    location varchar(255),
    contact char(11) not null,
    constraint unique_cnpj_seller unique (CNPJ),
    constraint unique_cpf_seller unique (CPF)
);

create TABLE productSeller(
	idPseller int,
    idProduct int,
    prodQuantify int default 1,
    PRIMARY KEY (idPseller, idProduct),
    CONSTRAINT fk_product_seller FOREIGN KEY(idPSeller) REFERENCES seller(idSeller),
    CONSTRAINT fk_product_product FOREIGN KEY (idProduct) REFERENCES product(idProduct)
);

create TABLE productOrder(
	idOproduct int,
    idPOorder int,
    poQuantify int default 1,
    poStatus enum('Disponível', 'Sem Estoque') default 'Disponível',
    PRIMARY KEY (idOproduct, idPOorder),
    CONSTRAINT fk_product_seller FOREIGN KEY(idOproduct) REFERENCES product(idProduct),
    CONSTRAINT fk_product_product FOREIGN KEY (idPOorder) REFERENCES orders(idOrder)
);

create TABLE storageLocation(
	idLproduct int,
    idLstorage int,
    location varchar(255) not null,
    primary key (idLproduct, idLstorage),
    CONSTRAINT fk_storage_location_product FOREIGN KEY (idLproduct) REFERENCES product(idProduct),
    CONSTRAINT fk_storage_location_storage FOREIGN KEY (idLstorage) REFERENCES productStorage(idProdStorage)
);
