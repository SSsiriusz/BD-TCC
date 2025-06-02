# BD-TCC
Banco de dados para utilizar no tcc

CREATE TABLE CategoriaDoacaoEnum (
    id INT AUTO_INCREMENT PRIMARY KEY,
    descricao VARCHAR(255) NOT NULL,
    categoria_doacao ENUM('ALIMENTO', 'ROUPA', 'HIGIENE', 'OUTROS') NOT NULL
);

;

CREATE TABLE StatusDoacaoEnum (
    id INT AUTO_INCREMENT PRIMARY KEY,
    descricao VARCHAR(255) NOT NULL,
    status_doacao ENUM('DISPONIVEL', 'RESERVADA', 'ENTREGUE', 'EXPIRADA') NOT NULL
);

;

CREATE TABLE StatusReservaEnum (
    id INT AUTO_INCREMENT PRIMARY KEY,
    descricao VARCHAR(255) NOT NULL,
    status_reserva ENUM('PENDENTE', 'CONFIRMADA', 'CANCELADA') NOT NULL
);

;

CREATE TABLE voluntarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    areaDeAtuacao VARCHAR(255) NOT NULL
);

;

CREATE TABLE ongs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cnpj VARCHAR(20) NOT NULL,
    areaAtuacao VARCHAR(255) NOT NULL
);

;

CREATE TABLE pessoas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cpf VARCHAR(15) NOT NULL,
    documentoComprovante VARCHAR(255) NOT NULL
);

;

CREATE TABLE comercios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cnpj VARCHAR(20) NOT NULL,
    nomeFantasia VARCHAR(255) NOT NULL
);

;

CREATE TABLE produtores_rurais (
    id INT AUTO_INCREMENT PRIMARY KEY,
    numeroRegistroRural VARCHAR(50) NOT NULL
);


;

CREATE TABLE geolocalizacoes (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,  
    latitude DOUBLE NOT NULL,              
    longitude DOUBLE NOT NULL,             
    cep VARCHAR(10) NOT NULL,              
    cidade VARCHAR(100) NOT NULL,          
    estado VARCHAR(2) NOT NULL             
);

;

CREATE TABLE Doacoes (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,                       
    descricao VARCHAR(255) NOT NULL,                              
    dataCadastro DATETIME NOT NULL,                               
    dataExpiracao DATETIME NOT NULL,                              
    categoria_id INT NOT NULL,                                    
    quantidade INT NOT NULL,                                      
    localizacao_id BIGINT NOT NULL,                               
    status1_id INT NOT NULL,                                      
    doador_id BIGINT NOT NULL,                                     
    reservadaPara_id BIGINT,                                      
    
    -- Definindo as chaves estrangeiras
    FOREIGN KEY (categoria_id) REFERENCES CategoriaDoacaoEnum(id),     
    FOREIGN KEY (localizacao_id) REFERENCES geolocalizacoes(id),        
    FOREIGN KEY (status1_id) REFERENCES StatusDoacaoEnum(id),          
    FOREIGN KEY (doador_id) REFERENCES Usuario(id),                   
    FOREIGN KEY (reservadaPara_id) REFERENCES Usuario(id)
);


;

CREATE TABLE Endereco (
    id INT AUTO_INCREMENT PRIMARY KEY,      
    rua VARCHAR(255) NOT NULL,              
    numero VARCHAR(10),                     
    bairro VARCHAR(100),                    
    cidade VARCHAR(100),                    
    estado VARCHAR(50),                     
    cep VARCHAR(10)                         
);

;

CREATE TABLE Usuario (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,   
    nome VARCHAR(255) NOT NULL,             
    email VARCHAR(255) NOT NULL UNIQUE,     
    senha VARCHAR(255) NOT NULL,            
    telefone VARCHAR(15),                   
    endereco_id INT,                        
    tipo ENUM('Admin', 'Cliente', 'Fornecedor') NOT NULL, 
    FOREIGN KEY (endereco_id) REFERENCES Endereco(id)   
);

;

CREATE TABLE DoacaoDAO (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,                            
    dataReserva DATETIME NOT NULL,                                    
    usuario_id BIGINT NOT NULL,                                       
    doacao_id BIGINT NOT NULL,                                        
    status_id INT NOT NULL,                                           
    
   
    FOREIGN KEY (usuario_id) REFERENCES Usuario(id),                 
    FOREIGN KEY (doacao_id) REFERENCES Doacoes(id),                   
    FOREIGN KEY (status_id) REFERENCES StatusReservaEnum(id)
    );



