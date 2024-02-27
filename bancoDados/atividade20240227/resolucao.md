-- --------------------------------------------------------
-- Servidor:                     127.0.0.1
-- Versão do servidor:           10.4.32-MariaDB - mariadb.org binary distribution
-- OS do Servidor:               Win64
-- HeidiSQL Versão:              12.6.0.6765
-- --------------------------------------------------------

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET NAMES utf8 */;
/*!50503 SET NAMES utf8mb4 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;


-- Copiando estrutura do banco de dados para escola
DROP DATABASE IF EXISTS `escola`;
CREATE DATABASE IF NOT EXISTS `escola` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci */;
USE `escola`;

-- Copiando estrutura para tabela escola.disciplinas
DROP TABLE IF EXISTS `disciplinas`;
CREATE TABLE IF NOT EXISTS `disciplinas` (
  `idDisciplina` int(11) NOT NULL AUTO_INCREMENT,
  `abreviacao` varchar(5) NOT NULL,
  `descricao` varchar(100) NOT NULL,
  `cargaHoraria` int(11) NOT NULL,
  PRIMARY KEY (`idDisciplina`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Copiando dados para a tabela escola.disciplinas: ~3 rows (aproximadamente)
DELETE FROM `disciplinas`;
INSERT INTO `disciplinas` (`idDisciplina`, `abreviacao`, `descricao`, `cargaHoraria`) VALUES
	(1, 'BD', 'Banco de dados', 100),
	(2, 'LR', 'Levantamento de requisitos', 200),
	(3, 'LPAL', 'Logica de Programacao e Algoritimo', 200);

-- Copiando estrutura para tabela escola.horarios_professor
DROP TABLE IF EXISTS `horarios_professor`;
CREATE TABLE IF NOT EXISTS `horarios_professor` (
  `idHorario` int(11) NOT NULL AUTO_INCREMENT,
  `idTurma` int(11) NOT NULL DEFAULT 0,
  `idPessoa` int(11) NOT NULL DEFAULT 0,
  `horaInicial` time NOT NULL,
  `horaFinal` time NOT NULL,
  `diaSemana` int(11) DEFAULT NULL COMMENT '0 domingo .. 1 segunda .. 2 terca ...',
  PRIMARY KEY (`idHorario`),
  KEY `FK__turmas` (`idTurma`),
  KEY `FK__pessoas` (`idPessoa`),
  CONSTRAINT `FK__pessoas` FOREIGN KEY (`idPessoa`) REFERENCES `pessoas` (`idPessoa`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `FK__turmas` FOREIGN KEY (`idTurma`) REFERENCES `turmas` (`idTurmas`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Copiando dados para a tabela escola.horarios_professor: ~6 rows (aproximadamente)
DELETE FROM `horarios_professor`;
INSERT INTO `horarios_professor` (`idHorario`, `idTurma`, `idPessoa`, `horaInicial`, `horaFinal`, `diaSemana`) VALUES
	(1, 4, 2, '19:00:00', '22:30:00', 1),
	(2, 1, 2, '08:00:00', '17:20:00', 1),
	(3, 1, 2, '08:00:00', '410:00:00', 2),
	(4, 5, 2, '08:00:00', '17:20:00', 3),
	(5, 1, 2, '08:00:00', '17:20:00', 4),
	(6, 5, 2, '08:00:00', '17:20:00', 5);

-- Copiando estrutura para tabela escola.matricula_alunos
DROP TABLE IF EXISTS `matricula_alunos`;
CREATE TABLE IF NOT EXISTS `matricula_alunos` (
  `idMatricula` int(11) NOT NULL AUTO_INCREMENT,
  `idPessoa` int(11) NOT NULL DEFAULT 0,
  `idTurma` int(11) NOT NULL DEFAULT 0,
  `dtInicio` date NOT NULL,
  `dtFim` date NOT NULL,
  PRIMARY KEY (`idMatricula`),
  KEY `FK__pessoas1` (`idPessoa`),
  KEY `FK__turmas1` (`idTurma`),
  CONSTRAINT `FK__pessoas1` FOREIGN KEY (`idPessoa`) REFERENCES `pessoas` (`idPessoa`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `FK__turmas1` FOREIGN KEY (`idTurma`) REFERENCES `turmas` (`idTurmas`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Copiando dados para a tabela escola.matricula_alunos: ~1 rows (aproximadamente)
DELETE FROM `matricula_alunos`;
INSERT INTO `matricula_alunos` (`idMatricula`, `idPessoa`, `idTurma`, `dtInicio`, `dtFim`) VALUES
	(1, 3, 4, '2024-02-27', '2024-03-10');

-- Copiando estrutura para tabela escola.pessoas
DROP TABLE IF EXISTS `pessoas`;
CREATE TABLE IF NOT EXISTS `pessoas` (
  `idPessoa` int(11) NOT NULL AUTO_INCREMENT,
  `nome` varchar(100) NOT NULL,
  `cpf` varchar(11) NOT NULL,
  `endereco` varchar(150) NOT NULL,
  `dtNasc` date NOT NULL,
  `tipoPessoa` int(11) NOT NULL,
  PRIMARY KEY (`idPessoa`),
  KEY `FK_pessoas_tipopessoa` (`tipoPessoa`),
  CONSTRAINT `FK_pessoas_tipopessoa` FOREIGN KEY (`tipoPessoa`) REFERENCES `tipo_pessoa` (`idTipo`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Copiando dados para a tabela escola.pessoas: ~5 rows (aproximadamente)
DELETE FROM `pessoas`;
INSERT INTO `pessoas` (`idPessoa`, `nome`, `cpf`, `endereco`, `dtNasc`, `tipoPessoa`) VALUES
	(2, 'Bruno Ferreira', '11111122256', 'rua teste 123 centro', '1988-06-16', 1),
	(3, 'Joao da silva ', '55566688899', 'rua tres ', '2004-02-27', 2),
	(4, 'Pedro', '5615631156', 'Rua dois', '1999-02-26', 2),
	(6, 'Maria', '1646816861', 'rua 4567', '1994-02-27', 2),
	(7, 'Kaua Silva', '156156115', 'Teste', '1998-02-27', 2);

-- Copiando estrutura para tabela escola.tipo_pessoa
DROP TABLE IF EXISTS `tipo_pessoa`;
CREATE TABLE IF NOT EXISTS `tipo_pessoa` (
  `idTipo` int(11) NOT NULL AUTO_INCREMENT,
  `descricao` varchar(50) NOT NULL,
  PRIMARY KEY (`idTipo`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Copiando dados para a tabela escola.tipo_pessoa: ~2 rows (aproximadamente)
DELETE FROM `tipo_pessoa`;
INSERT INTO `tipo_pessoa` (`idTipo`, `descricao`) VALUES
	(1, 'Professor'),
	(2, 'Aluno');

-- Copiando estrutura para tabela escola.turmas
DROP TABLE IF EXISTS `turmas`;
CREATE TABLE IF NOT EXISTS `turmas` (
  `idTurmas` int(11) NOT NULL AUTO_INCREMENT,
  `descricao` varchar(50) NOT NULL,
  `periodo` varchar(8) NOT NULL,
  `unidade` varchar(100) NOT NULL,
  PRIMARY KEY (`idTurmas`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Copiando dados para a tabela escola.turmas: ~5 rows (aproximadamente)
DELETE FROM `turmas`;
INSERT INTO `turmas` (`idTurmas`, `descricao`, `periodo`, `unidade`) VALUES
	(1, 'DEV3A', 'Diurno', 'Presidente Prudente'),
	(2, 'DEV3A', 'Diruno', 'Santo Anastacio'),
	(3, 'DEV3B', 'Diruno', 'Presidente Prudente'),
	(4, 'AZ900', 'Noite', 'Presidente Prudente'),
	(5, 'DEV2A', 'Diurno', 'Presidente Prudente');

/*!40103 SET TIME_ZONE=IFNULL(@OLD_TIME_ZONE, 'system') */;
/*!40101 SET SQL_MODE=IFNULL(@OLD_SQL_MODE, '') */;
/*!40014 SET FOREIGN_KEY_CHECKS=IFNULL(@OLD_FOREIGN_KEY_CHECKS, 1) */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40111 SET SQL_NOTES=IFNULL(@OLD_SQL_NOTES, 1) */;
