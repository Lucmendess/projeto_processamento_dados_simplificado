# projeto_processamento_dados_simplificado

O projeto visa a implementação de um processo simplificado de coleta, transformação e análise de dados utilizando Power BI e uma base de dados MySQL hospedada no Azure. O foco é otimizar a visualização e a análise de dados relacionados a colaboradores e suas respectivas estruturas de gerenciamento e departamentos.

Etapas do Projeto
Criação da Infraestrutura no Azure

Configuração de uma instância de banco de dados MySQL no Azure para armazenar os dados.
Importação da base de dados disponível em um repositório do GitHub.
Integração com Power BI

Estabelecimento de conexão entre Power BI e a instância MySQL.
Importação de tabelas relevantes para análise.
Análise e Transformação dos Dados

Verificação de cabeçalhos e tipos de dados.
Conversão de valores monetários para tipo double.
Identificação e tratamento de valores nulos, incluindo colaboradores sem gerente e departamentos sem gerenciamento.
Agrupamento dos dados para contar colaboradores por gerente.
Remoção de colunas desnecessárias que não contribuirão para o relatório final.
Modelagem de Dados

Mesclagem de tabelas de colaboradores e departamentos para criar uma visão unificada dos colaboradores e suas associações.
Criação de colunas combinadas para facilitar a análise, como a combinação de nomes e sobrenomes e a união de departamentos e localizações.
Geração de Relatórios

Desenvolvimento de visualizações no Power BI para apresentar insights sobre a estrutura da equipe, desempenho de colaboradores e outros KPIs relevantes.
Compartilhamento dos relatórios com stakeholders para tomada de decisões.
Resultados Esperados
Um conjunto de dados limpo e estruturado que facilita a análise.
Relatórios visuais que oferecem insights sobre a força de trabalho e sua gestão.
Melhoria na eficiência do processo de tomada de decisões baseadas em dados.
Benefícios
Aumento da eficiência na análise de dados.
Melhor compreensão da estrutura organizacional.
Informações valiosas para otimização de recursos humanos e gestão de equipes.
 
  - Usando as seguintes consultas e queries no Power BI, onde foi mesclando tabelas como departamentos_loc_Stafford e departamento_funcionario:
    -- Consultas SQL

select * from employee;
select Ssn, count(Essn) from employee e, dependent d where (e.Ssn = d.Essn);
select * from dependent;

SELECT Bdate, Address FROM employee
WHERE Fname = 'John' AND Minit = 'B' AND Lname = 'Smith';

select * from departament where Dname = 'Research';

SELECT Fname, Lname, Address
FROM employee, departament
WHERE Dname = 'Research' AND Dnumber = Dno;

select * from project;
--
--
--
-- Expressões e concatenação de strings
--
--
-- recuperando informações dos departamentos presentes em Stafford
select Dname as Department, Mgr_ssn as Manager from departament d, dept_locations l
where d.Dnumber = l.Dnumber;

-- padrão sql -> || no MySQL usa a função concat()
select Dname as Department, concat(Fname, ' ', Lname) from departament d, dept_locations l, employee e
where d.Dnumber = l.Dnumber and Mgr_ssn = e.Ssn;

-- recuperando info dos projetos em Stafford
select * from project, departament where Dnum = Dnumber and Plocation = 'Stafford';

-- recuperando info sobre os departamentos e projetos localizados em Stafford
SELECT Pnumber, Dnum, Lname, Address, Bdate
FROM project, departament, employee
WHERE Dnum = Dnumber AND Mgr_ssn = Ssn AND
Plocation = 'Stafford';

SELECT * FROM employee WHERE Dno IN (3,6,9);

--
--
-- Operadores lógicos
--
--

SELECT Bdate, Address
FROM EMPLOYEE
WHERE Fname = ‘John’ AND Minit = ‘B’ AND Lname = ‘Smith’;

SELECT Fname, Lname, Address
FROM EMPLOYEE, DEPARTMENT
WHERE Dname = ‘Research’ AND Dnumber = Dno;

--
--
-- Expressões e alias
--
--

-- recolhendo o valor do INSS-*
select Fname, Lname, Salary, Salary*0.011 from employee;
select Fname, Lname, Salary, Salary*0.011 as INSS from employee;
select Fname, Lname, Salary, round(Salary*0.011,2) as INSS from employee;

-- definir um aumento de salário para os gerentes que trabalham no projeto associado ao ProdutoX
select e.Fname, e.Lname, 1.1*e.Salary as increased_sal from employee as e,
works_on as w, project as p where e.Ssn = w.Essn and w.Pno = p.Pnumber and p.Pname='ProductX';

-- concatenando e fornecendo alias
select Dname as Department, concat(Fname, ' ', Lname) as Manager from departament d, dept_locations l, employee e
where d.Dnumber = l.Dnumber and Mgr_ssn = e.Ssn;

-- recuperando dados dos empregados que trabalham para o departamento de pesquisa
select Fname, Lname, Address from employee, departament
	where Dname = 'Research' and Dnumber = Dno;

-- definindo alias para legibilidade da consulta
select e.Fname, e.Lname, e.Address from employee e, departament d
	where d.Dname = 'Research' and d.Dnumber = e.Dno;



