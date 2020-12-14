# Descoberta de Conhecimento com Python

Este repositório faz referência à um trabalho sobre KDD (Knowledge Discovery in Databases) da disciplina de Banco de Dados II. Durante o trabalho foram abordadas todas as etapas de KDD, tendo como base de dados um arquivo com pedidos de uma pizzaria.


<h2> Seleção </h2>

SQL para criação do banco pizzaria e da tabela pedidos_full. Os dados para inserção podem ser importados através do <b>.csv</b> disponibilizado na pasta <b>selecao</b> ou através do SQL de inserção contido do arquivo <b>database.sql</b>

```
CREATE DATABASE  IF NOT EXISTS `pizzaria`;
USE `pizzaria`;

--
-- Table structure for table `pedidos`
--

DROP TABLE IF EXISTS `pedidos_full`;

CREATE TABLE `pedidos_full` (
  `numero` int(11) NOT NULL,
  `data_pedido` date DEFAULT NULL,
  `hora_pedido` time DEFAULT NULL,
  `cliente` varchar(20) DEFAULT NULL,
  `endereco` varchar(20) DEFAULT NULL,
  `telefone` varchar(20) DEFAULT NULL,
  `tipo_entrega` varchar(20) DEFAULT NULL,
  `valor_pizza` float DEFAULT NULL,
  `valor_borda` float DEFAULT NULL,
  `valor_refrigerante` float DEFAULT NULL,
  `valor_entrega` float DEFAULT NULL,
  `valor_total` float DEFAULT NULL,
  `hora_entrega` time DEFAULT NULL,
  `tempo` time DEFAULT NULL,
  PRIMARY KEY (`numero`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```
<h2> Transformação </h2>

<ul>
  <li>Descarte de colunas inutilizáveis</li>
</ul>
```
ALTER TABLE pedidos_full DROP COLUMN numero;
ALTER TABLE pedidos_full DROP COLUMN cliente;
ALTER TABLE pedidos_full DROP COLUMN endereco;
ALTER TABLE pedidos_full DROP COLUMN telefone;
ALTER TABLE pedidos_full DROP COLUMN valor_pizza;
ALTER TABLE pedidos_full DROP COLUMN valor_entrega;
ALTER TABLE pedidos_full DROP COLUMN hora_entrega;
```

<ul>
  <li>
    Histogramas gerados para função de transformação de valor e tempo
    <img src="numpy-matplotlib/dados1.png">
  </li>

</ul>

```
/*importar o transforma_valor*/
SELECT transforma_valor(valor_total) from pedidos_full;

/*transforma valor*/
DELIMITER $$
CREATE FUNCTION transforma_valor(valor_total float) 
RETURNS varchar(20)

BEGIN
    DECLARE preco varchar(20);
    IF(valor_total>= 10 AND valor_total <= 13) THEN
        SET preco = 'vl 10-13';
    ELSEIF(valor_total> 13 AND valor_total <= 16) THEN
        SET preco = 'vl 13-16';
    ELSEIF(valor_total> 16 AND valor_total <= 20) THEN
        SET preco = 'vl 16-20';
    ELSEIF(valor_total> 20 AND valor_total <= 24) THEN
        SET preco = 'vl 20-24';
    ELSEIF(valor_total> 24 AND valor_total <= 28) THEN
        SET preco = 'vl 24-28';    
    ELSEIF(valor_total> 28 AND valor_total <= 32) THEN
        SET preco = 'vl 28-32'; 
    ELSEIF(valor_total> 32 AND valor_total <= 36) THEN
        SET preco = 'vl 32-36';
    ELSEIF(valor_total> 36 AND valor_total <= 40) THEN
        SET preco = 'vl 36-40';
    ELSEIF(valor_total> 40 AND valor_total <= 44) THEN
        SET preco = 'vl 40-44';
    ELSEIF(valor_total> 44 AND valor_total <= 50) THEN
        SET preco = 'vl 44-50';
    END IF;
    RETURN preco;

END $$

delimiter;
```

