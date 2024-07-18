# Documentação para a Avaliação de SQL

---

### 1. Desenho da Base de Dados

Para a realização deste projeto, foi utilizado a ferramenta de apoio "Mysql Workbench" e a "AWS RDS" para o host do Mysql. 
![imagem](https://github.com/user-attachments/assets/667ed1e3-cc07-469f-882c-08f678174d63)

---

### 2. Criação da Base de Dados

Nesta etapa, foi feito a criação da base de dados seguindo o modelo atribuido, fazendo uma pequena alteração na tabela "Bilhetes", onde foi realizado 
uma relação com a tabela "Funcionários" para que possamos saber qual funcionário vendeu certo bilhete para qual cliente.

```sql
-- Desativar verificações de chave única
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;

-- Desativar verificações de chave estrangeira
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;

-- Configurar modo SQL para o processo
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema Cinema
-- -----------------------------------------------------

-- Remover o esquema se ele existir
DROP SCHEMA IF EXISTS `Cinema`;

-- Criar o esquema se ele não existir
CREATE SCHEMA IF NOT EXISTS `Cinema`;

-- Usar o esquema Cinema
USE `Cinema`;

-- -----------------------------------------------------
-- Table `Cinema`.`Filmes`
-- -----------------------------------------------------

-- Criar tabela Filmes
CREATE TABLE IF NOT EXISTS `Cinema`.`Filmes` (
`ID` INT NOT NULL AUTO_INCREMENT,
`Título` VARCHAR(45) NOT NULL,
`Diretor` VARCHAR(45) NOT NULL,
`Género` VARCHAR(45) NOT NULL,
`Duração` VARCHAR(6) NOT NULL,
`Classificação` VARCHAR(45) NOT NULL,
PRIMARY KEY (`ID`))
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Table `Cinema`.`Clientes`
-- -----------------------------------------------------

-- Criar tabela Clientes
CREATE TABLE IF NOT EXISTS `Cinema`.`Clientes` (
`ID` INT NOT NULL AUTO_INCREMENT,
`Nome` VARCHAR(45) NOT NULL,
`Email` VARCHAR(45) NOT NULL,
`Telefone` INT(9) NOT NULL,
PRIMARY KEY (`ID`))
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Table `Cinema`.`Funcionários`
-- -----------------------------------------------------

-- Criar tabela Funcionários
CREATE TABLE IF NOT EXISTS `Cinema`.`Funcionários` (
`ID` INT NOT NULL AUTO_INCREMENT,
`Nome` VARCHAR(45) NOT NULL,
`Posição` VARCHAR(45) NOT NULL,
`Salário` VARCHAR(45) NOT NULL,
PRIMARY KEY (`ID`))
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Table `Cinema`.`Sessões`
-- -----------------------------------------------------

-- Criar tabela Sessões
CREATE TABLE IF NOT EXISTS `Cinema`.`Sessões` (
`ID` INT NOT NULL AUTO_INCREMENT,
`FilmeID` INT NOT NULL,
`DataHora` DATETIME NOT NULL,
`Sala` VARCHAR(45) NOT NULL,
`LugaresDisponiveis` INT NOT NULL,
PRIMARY KEY (`ID`, `FilmeID`),
INDEX `fk_Sessões_Filmes_idx` (`FilmeID` ASC) VISIBLE,
CONSTRAINT `fk_Sessões_Filmes`
FOREIGN KEY (`FilmeID`)
REFERENCES `Cinema`.`Filmes` (`ID`)
ON DELETE NO ACTION
ON UPDATE NO ACTION)
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Table `Cinema`.`Bilhetes`
-- -----------------------------------------------------

-- Criar tabela Bilhetes
CREATE TABLE IF NOT EXISTS `Cinema`.`Bilhetes` (
`ID` INT NOT NULL AUTO_INCREMENT,
`FuncionárioID` INT NOT NULL,
`ClienteID` INT NOT NULL,
`SessãoID` INT NOT NULL,
`Quantidade` VARCHAR(45) NOT NULL,
`Preço` DECIMAL(5,2) NOT NULL,
PRIMARY KEY (`ID`, `FuncionárioID`, `ClienteID`, `SessãoID`),
INDEX `fk_Bilhetes_Funcionários1_idx` (`FuncionárioID` ASC) VISIBLE,
INDEX `fk_Bilhetes_Clientes1_idx` (`ClienteID` ASC) VISIBLE,
INDEX `fk_Bilhetes_Sessões1_idx` (`SessãoID` ASC) VISIBLE,
CONSTRAINT `fk_Bilhetes_Funcionários1`
FOREIGN KEY (`FuncionárioID`)
REFERENCES `Cinema`.`Funcionários` (`ID`)
ON DELETE NO ACTION
ON UPDATE NO ACTION,
CONSTRAINT `fk_Bilhetes_Clientes1`
FOREIGN KEY (`ClienteID`)
REFERENCES `Cinema`.`Clientes` (`ID`)
ON DELETE NO ACTION
ON UPDATE NO ACTION,
CONSTRAINT `fk_Bilhetes_Sessões1`
FOREIGN KEY (`SessãoID`)
REFERENCES `Cinema`.`Sessões` (`ID`)
ON DELETE NO ACTION
ON UPDATE NO ACTION)
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Table `Cinema`.`messages`
-- -----------------------------------------------------

-- Criar tabela messages
CREATE TABLE IF NOT EXISTS `Cinema`.`messages` (
`message` VARCHAR(45))
ENGINE = InnoDB;

-- Restaurar configurações anteriores de modo SQL
SET SQL_MODE=@OLD_SQL_MODE;

-- Restaurar configurações anteriores de verificação de chave estrangeira
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;

-- Restaurar configurações anteriores de verificação de chave única
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
```
---

### 3. Inserção de Dados 

Seguidamente à criação da base de dados, podemos inserimos uma certa quantidade de dados.

```sql
-- Inserir registros na tabela Filmes
INSERT INTO `Cinema`.`Filmes` (`Título`, `Diretor`, `Género`, `Duração`, `Classificação`) VALUES
('The Great Adventure', 'John Smith', 'Ação', '02:00', 'M/12'),
('Romantic Escape', 'Jane Doe', 'Romance', '01:45', 'M/6'),
('Horror Night', 'Eli Roth', 'Terror', '01:30', 'M/18'),
('The Animated Journey', 'Pixar', 'Animação', '01:20', 'M/3'),
('Science Future', 'Steven Spielberg', 'Ficção Científica', '02:30', 'M/12'),
('Family Tales', 'Chris Columbus', 'Comédia', '01:50', 'M/6'),
('Mystery of the Lost City', 'Agatha Christie', 'Mistério', '02:10', 'M/14'),
('Space Odyssey', 'Stanley Kubrick', 'Ficção Científica', '02:20', 'M/12'),
('Historical Drama', 'Ken Burns', 'Drama', '02:00', 'M/12'),
('Epic Saga', 'George Lucas', 'Aventura', '02:30', 'M/16');

-- Inserir registros na tabela Funcionários
INSERT INTO `Cinema`.`Funcionários` (`Nome`, `Posição`, `Salário`) VALUES
('Ana Silva', 'Caixa', '1200'),
('Carlos Sousa', 'Gerente', '2500'),
('Maria Oliveira', 'Atendente', '1100'),
('Pedro Martins', 'Caixa', '1200'),
('João Santos', 'Segurança', '1300'),
('Luís Fernandes', 'Projecionista', '1400'),
('Mariana Costa', 'Atendente', '1100'),
('Fernanda Lima', 'Caixa', '1200'),
('Rafael Alves', 'Segurança', '1300'),
('Tiago Rocha', 'Projecionista', '1400');

-- Inserir registros na tabela Clientes
INSERT INTO `Cinema`.`Clientes` (`Nome`, `Email`, `Telefone`) VALUES
('Lucas Pereira', 'lucas.pereira@mail.com', 912345678),
('Gabriela Lima', 'gabriela.lima@mail.com', 919876543),
('Ricardo Silva', 'ricardo.silva@mail.com', 918273645),
('Juliana Costa', 'juliana.costa@mail.com', 917364258),
('André Santos', 'andre.santos@mail.com', 916485372),
('Mariana Gomes', 'mariana.gomes@mail.com', 915294736),
('Fernando Alves', 'fernando.alves@mail.com', 914857362),
('Sofia Rodrigues', 'sofia.rodrigues@mail.com', 913746582),
('Paulo Ferreira', 'paulo.ferreira@mail.com', 912358476),
('Ana Sousa', 'ana.sousa@mail.com', 911764382);

-- Inserir registros na tabela Sessões
INSERT INTO `Cinema`.`Sessões` (`FilmeID`, `DataHora`, `Sala`, `LugaresDisponiveis`) VALUES
(1, '2024-07-16 18:00:00', 'Sala 1', 100),
(2, '2024-07-16 20:30:00', 'Sala 2', 75),
(3, '2024-07-17 22:00:00', 'Sala 3', 90),
(4, '2024-07-18 16:15:00', 'Sala 1', 100),
(5, '2024-07-19 18:45:00', 'Sala 2', 60),
(6, '2024-07-20 21:00:00', 'Sala 3', 50),
(7, '2024-07-21 17:30:00', 'Sala 1', 100),
(8, '2024-07-22 19:00:00', 'Sala 2', 85),
(9, '2024-07-23 20:45:00', 'Sala 3', 70),
(10, '2024-07-24 15:00:00', 'Sala 1', 100),
(1, '2024-07-25 17:15:00', 'Sala 2', 90),
(2, '2024-07-26 20:00:00', 'Sala 3', 75),
(3, '2024-07-27 22:30:00', 'Sala 4', 65),
(4, '2024-07-28 16:45:00', 'Sala 1', 100),
(5, '2024-07-29 19:15:00', 'Sala 2', 55),
(6, '2024-07-30 21:30:00', 'Sala 3', 80),
(7, '2024-07-31 18:00:00', 'Sala 1', 100),
(8, '2024-08-01 20:45:00', 'Sala 2', 70),
(9, '2024-08-02 23:00:00', 'Sala 3', 90),
(10, '2024-08-03 15:30:00', 'Sala 1', 100),
(1, '2024-08-04 18:00:00', 'Sala 2', 80),
(2, '2024-08-05 20:30:00', 'Sala 3', 75),
(3, '2024-08-06 17:00:00', 'Sala 1', 100),
(4, '2024-08-07 19:30:00', 'Sala 2', 60),
(5, '2024-08-08 22:00:00', 'Sala 3', 85),
(2, '2024-08-08 23:00:00', 'Sala 2', 1);

-- Inserir registros na tabela Bilhetes
INSERT INTO `Cinema`.`Bilhetes` (`FuncionárioID`, `ClienteID`, `SessãoID`, `Quantidade`, `Preço`) VALUES
(2, 5, 1, '2', 15.00),
(1, 3, 2, '1', 7.50),
(8, 4, 3, '3', 22.50),
(6, 2, 4, '4', 30.00),
(10, 1, 5, '2', 15.00),
(3, 6, 6, '1', 7.50),
(5, 8, 7, '3', 22.50),
(4, 10, 8, '4', 30.00),
(7, 9, 9, '2', 15.00),
(2, 7, 10, '3', 22.50),
(9, 2, 11, '1', 7.50),
(6, 5, 12, '4', 30.00),
(8, 3, 13, '2', 15.00),
(1, 9, 14, '3', 22.50),
(10, 4, 15, '1', 7.50),
(7, 1, 16, '4', 30.00),
(5, 6, 17, '2', 15.00),
(4, 8, 18, '3', 22.50),
(3, 10, 19, '1', 7.50),
(9, 7, 20, '4', 30.00),
(2, 5, 21, '3', 22.50),
(6, 3, 22, '2', 15.00),
(8, 9, 23, '1', 7.50),
(1, 4, 24, '4', 30.00),
(10, 2, 25, '3', 22.50);

-- Selecionar todos os registros da tabela Bilhetes
SELECT * FROM `Cinema`.`Bilhetes`;

-- Selecionar todos os registros da tabela Clientes
SELECT * FROM `Cinema`.`Clientes`;

-- Selecionar todos os registros da tabela Filmes
SELECT * FROM `Cinema`.`Filmes`;

-- Selecionar todos os registros da tabela Funcionários
SELECT * FROM `Cinema`.`Funcionários`;

-- Selecionar todos os registros da tabela Sessões
SELECT * FROM `Cinema`.`Sessões`;
```
---

### 4. Consultas SQL

#### 4.1 Aqui podemos listar todos os filmes com os respetivos diretores e géneros

```sql
-- Selecionar as colunas Título, Diretor e Género da tabela Filmes
SELECT Título, Diretor, Género FROM Filmes;
```

#### 4.2 Aqui iremos listar todos as sessões realizadas num determinado periodo

```sql
-- Selecionar todas as colunas da tabela Sessões onde a DataHora está entre 'DataInicial' e 'DataFinal'
SELECT * FROM Sessões
WHERE DataHora BETWEEN 'DataInicial' AND 'DataFinal';


Exemplo:

Select * from Sessões
where DataHora between '2024-07-20' and '2024-07-28'
```

#### 4.3 Aqui listaremos todos os clientes que compraram mais do que um determinado número de Bilhetes

```sql
-- Seleciona o nome do cliente, a quantidade e o preço dos bilhetes para bilhetes em que a quantidade comprada é maior do que a quantidade especificada
SELECT Clientes.Nome, Quantidade, Preço FROM Bilhetes
JOIN Clientes USING (ID)
WHERE Quantidade > 'Quantidade que desejar';


Exemplo:

Select Clientes.Nome, Quantidade, Preço from Bilhetes
join Clientes using (ID)
Where Quantidade > '2'
```

#### 4.4 Aqui vamos listar todos os Filmes de um determinado género

```sql
-- Seleciona todos os registros da tabela Filmes onde o gênero corresponde ao especificado
Select * from Filmes
where Género = 'Género que escolher'

Exemplo:

Select * from Filmes
where Género = 'Ficção Cientifica'
```
---

### 5.Triggers

#### 5.1 Aqui crio um trigger que atualiza a quantidade de lugares disponiveis numa sessão após a compra de um bilhete.


```sql
-- Remove o trigger 'Update_Lugares_Disponiveis' caso ele já exista, para evitar erros ao criar o novo trigger
DROP TRIGGER IF EXISTS Update_Lugares_Disponiveis;

-- Define o delimitador como // para permitir o uso de múltiplas instruções SQL no corpo do trigger
DELIMITER //

-- Cria um novo trigger chamado 'update_lugares_disponiveis'
-- Este trigger é executado após a inserção de uma nova linha na tabela 'Bilhetes'
CREATE TRIGGER update_lugares_disponiveis
    AFTER INSERT ON Bilhetes
    FOR EACH ROW
BEGIN
    -- Declara uma variável chamada 'mensagem' do tipo VARCHAR com tamanho de 45 caracteres
    DECLARE mensagem VARCHAR(45);

    -- Insere uma mensagem na tabela 'messages' indicando que o trigger 'update_lugares_disponiveis' foi acionado
    INSERT INTO messages(message) VALUES('update_lugares_disponiveis');

    -- Atualiza a tabela 'Sessões' para diminuir o número de 'LugaresDisponiveis'
    -- Subtrai o valor de 'NEW.Quantidade' do campo 'LugaresDisponiveis' para a sessão específica
    -- 'NEW' refere-se à nova linha que foi inserida na tabela 'Bilhetes'
    -- 'NEW.SessãoID' refere-se ao ID da sessão associada à nova linha inserida
    UPDATE Sessões
    SET LugaresDisponiveis = LugaresDisponiveis - NEW.Quantidade
    WHERE ID = NEW.SessãoID;
END;
//

-- Retorna o delimitador ao padrão (;)
DELIMITER ;
```

#### 5.2 Aqui crio um trigger que impede a compra de um bilhete caso a sessão escolhida não tiver lugares disponiveis suficientes.

```sql
-- Remove o trigger 'insert_bilhetes' caso ele já exista, para evitar erros ao criar o novo trigger
DROP TRIGGER IF EXISTS insert_bilhetes;

-- Define o delimitador como // para permitir o uso de múltiplas instruções SQL no corpo do trigger
DELIMITER //

-- Cria um novo trigger chamado 'insert_bilhetes'
-- Este trigger é executado antes da inserção de uma nova linha na tabela 'Bilhetes'
CREATE TRIGGER insert_bilhetes
    BEFORE INSERT ON Bilhetes
    FOR EACH ROW
BEGIN
    -- Declara uma variável chamada 'mensagem' do tipo VARCHAR com tamanho de 45 caracteres
    DECLARE mensagem VARCHAR(45);

    -- Declara uma variável chamada 'lugares_disponiveis' do tipo INT para armazenar o número de lugares disponíveis
    DECLARE lugares_disponiveis INT;

    -- Insere uma mensagem na tabela 'messages' indicando que o trigger 'insert_bilhetes' foi acionado
    INSERT INTO messages(message) VALUES('insert_bilhetes');

    -- Seleciona o número de 'LugaresDisponiveis' da tabela 'Sessões' para a sessão específica que está sendo inserida
    -- Armazena o resultado na variável 'lugares_disponiveis'
    SELECT LugaresDisponiveis INTO lugares_disponiveis FROM Sessões
    WHERE ID = NEW.SessãoID;

    -- Verifica se os 'lugares_disponiveis' são menores que a 'NEW.Quantidade' da nova linha
    -- Se houver menos lugares disponíveis do que a quantidade desejada, executa o bloco de código abaixo
    IF lugares_disponiveis < NEW.Quantidade THEN

        -- Cria uma mensagem de erro concatenando o texto com o valor de 'NEW.Quantidade'
        SET mensagem = CONCAT('Não existe ', NEW.Quantidade, ' vaga para esta sessão');

        -- Gera um erro SQL com um estado de erro definido ('45000') e a mensagem especificada
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = mensagem;
END IF;
END;
//

-- Retorna o delimitador ao padrão (;)
DELIMITER ;
```
---

### 6. Stored Procedures

#### 6.1 Aqui crio um stored procedure em que registra uma nova compra de bilhetes.
Nota: As variáveis dentro de "call Insert_Bilhetes()", para funcionar o comando, manualmente tem de alterar para o que deseja inserir na tabela 'Bilhetes'.

```sql
-- Remove o procedimento armazenado 'Insert_Bilhetes' caso ele já exista, para evitar erros ao criar o novo procedimento.
DROP PROCEDURE IF EXISTS Insert_Bilhetes;
-- Define o delimitador como $$ para permitir o uso de múltiplas instruções SQL no corpo do procedimento.
DELIMITER $$
-- Cria um novo procedimento armazenado chamado 'Insert_Bilhetes'.
-- Este procedimento insere um novo registro na tabela 'Bilhetes' com os valores fornecidos.
CREATE PROCEDURE `Insert_Bilhetes`(in _Funcionario int, _Cliente int, _Sessao int, _Quantidade int, _Preco Decimal(5,2))
BEGIN
    -- Insere um novo registro na tabela 'Bilhetes' usando os valores passados como parâmetros.
    insert into Bilhetes (FuncionárioID, ClienteID, SessãoID, Quantidade, Preço) values (_Funcionario, _Cliente, _Sessao, _Quantidade, _Preco);
END$$
-- Retorna o delimitador ao padrão (;)
DELIMITER ;

-- Chama o procedimento armazenado 'Insert_Bilhetes' para inserir um novo registro na tabela 'Bilhetes'.
CALL Insert_Bilhetes(_Funcionario, _Cliente, _Sessao, _Quantidade, _Preco);
```

#### 6.2 Aqui crio um stored procedure em que atualiza os dados de um cliente.
Nota: As variáveis dentro de "call update_cliente()", para funcionar o comando, manualmente tem de alterar para o que deseja atualizar na tabela 'Clientes'.


```sql
-- Drop do procedimento update_cliente se já existir, para evitar conflitos.
DROP PROCEDURE IF EXISTS update_cliente;

-- Mudança do delimitador para $$ para que o corpo do procedimento seja interpretado corretamente.
DELIMITER $$

-- Criação do procedimento update_cliente com parâmetros de entrada _ID, _Nome, _Email, _Telefone.
CREATE PROCEDURE `update_cliente`(in _ID int, _Nome varchar(45), _Email varchar(50), _Telefone int(9))
BEGIN
    -- Comando SQL que atualiza os dados na tabela Clientes.
update Clientes set Nome = _Nome, Email = _Email, Telefone = _Telefone
-- Condição para atualizar apenas o registro com o ID especificado.
where ID = _ID;
-- Fim da definição do procedimento, usando o novo delimitador $$.
END$$  

-- Restauração do delimitador padrão (;) para comandos SQL.
DELIMITER ;

-- Chamada do procedimento update_cliente com os parâmetros desejados.
call update_cliente(_ID, _Nome, _Email, _Telefone);

```


#### 6.3 Aqui crio um stored procedure em que calcula o total de bilhetes vendidos num determinado periodo de tempo.
Nota: As variáveis dentro de "call total_bilhetes()", para funcionar o comando, tem de alterar manualmente para o que desejar.

```sql
-- Drop do procedimento total_bilhetes se já existir, para evitar conflitos.
DROP PROCEDURE IF EXISTS total_bilhetes;

-- Mudança do delimitador para $$ para que o corpo do procedimento seja interpretado corretamente.
DELIMITER $$

-- Criação do procedimento total_bilhetes com parâmetros de entrada _Data1 e _Data2.
CREATE PROCEDURE `total_bilhetes`(in _Data1 DateTime, _Data2 DateTime)
BEGIN
-- Seleciona a soma da quantidade de bilhetes vendidos dentro do intervalo de datas especificado.
select sum(Quantidade) as totalbilhetes from Bilhetes
-- Junta a tabela Bilhetes com a tabela Sessões usando o ID da sessão.
join Sessões using (ID)
-- Condição para selecionar apenas as sessões que ocorrem entre as datas _Data1 e _Data2.
where DataHora between _Data1 and _Data2;
-- Fim da definição do procedimento, usando o novo delimitador $$.
END$$  

-- Restauração do delimitador padrão (;) para comandos SQL.
DELIMITER ;

-- Chamada do procedimento total_bilhetes com os parâmetros desejados _Data1 e _Data2.
call total_bilhetes(_Data1, _Data2);

```
---

### 7. Cursores

#### 7.1 Aqui crio um cursor que faz um relatório que lista todos os filmes exibidos juntamente com a quantidade de bilhetes vendido num determinado periodo de tempo.
Nota: As variáveis dentro de "call Relatório_Filmes()", para funcionar o comando, tem de alterar manualmente para o que desejar.


```sql
-- Criação da tabela temporária TempResults, se não existir, para armazenar os resultados temporários.
CREATE TEMPORARY TABLE IF NOT EXISTS TempResults (
    Título VARCHAR(45),
    Género VARCHAR(45),
    Quantidade INT
);

-- Drop do procedimento Relatório_Filmes, se já existir, para evitar conflitos.
DROP PROCEDURE IF EXISTS Relatório_Filmes;

-- Mudança do delimitador para $$ para que o corpo do procedimento seja interpretado corretamente.
DELIMITER $$

-- Criação do procedimento Relatório_Filmes com parâmetros de entrada _data1 e _data2.
CREATE PROCEDURE `Relatório_Filmes`( in _data1 datetime, _data2 datetime )
BEGIN
    -- Declaração de uma variável de controle para o cursor.
    DECLARE done INT DEFAULT FALSE;
    
    -- Declaração das variáveis locais para armazenar os resultados do cursor.
    DECLARE _Titulo varchar(45);
    DECLARE _Género varchar(45);
    DECLARE _Quantidade int;

    -- Declaração do cursor que seleciona os títulos dos filmes, seus gêneros e a quantidade de bilhetes vendidos dentro do intervalo de datas especificado.
    DECLARE cursor1 CURSOR FOR
SELECT Filmes.Título, Filmes.Género, SUM(Bilhetes.Quantidade) as Quantidade
FROM Bilhetes
         JOIN Sessões ON Bilhetes.SessãoID = Sessões.ID
         JOIN Filmes ON Sessões.FilmeID = Filmes.ID
WHERE DataHora BETWEEN _data1 AND _data2
GROUP BY Filmes.Título;

-- Tratamento de exceção para o cursor quando não há mais linhas para serem lidas.
DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;

    -- Abertura do cursor1 para iniciar a leitura das linhas selecionadas.
OPEN cursor1;

-- Loop para ler todas as linhas selecionadas pelo cursor e inserir os resultados na tabela temporária TempResults.
read_loop: LOOP
        FETCH cursor1 INTO _Titulo, _Género, _Quantidade;
        
        -- Verifica se não há mais linhas para serem lidas e sai do loop.
        IF done THEN
            LEAVE read_loop;
END IF;
        
        -- Insere os valores atuais de _Titulo, _Género e _Quantidade na tabela temporária TempResults.
INSERT INTO TempResults (Título, Género, Quantidade)
VALUES (_Titulo, _Género, _Quantidade);
END LOOP;

    -- Fecha o cursor após o término da leitura.
CLOSE cursor1;

-- Seleciona todos os registros da tabela temporária TempResults e os exibe como resultado do procedimento.
SELECT * FROM TempResults;

-- Drop da tabela temporária TempResults após o uso para liberar espaço.
DROP TEMPORARY TABLE IF EXISTS TempResults;
END$$  -- Fim da definição do procedimento, usando o novo delimitador $$.

-- Restauração do delimitador padrão (;) para comandos SQL.
DELIMITER ;

-- Chamada do procedimento Relatório_Filmes com os parâmetros desejados _data1 e _data2.
CALL Relatório_Filmes(_data1, _data2);
```
