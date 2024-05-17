# -Personalizando-Acessos-e-Automatizando-a-es-no-MySQL

-- Visão para o número de funcionários por departamento e localidade
CREATE VIEW visao_funcionarios_departamento_localidade AS
SELECT departamento, localidade, COUNT(*) AS quantidade_funcionarios
FROM empregados
GROUP BY departamento, localidade;

-- Visão para a lista de departamentos e seus gerentes
CREATE VIEW visao_departamentos_gerentes AS
SELECT departamento, gerente
FROM departamentos;

-- Visão para os projetos com maior número de funcionários
CREATE VIEW visao_projetos_maior_quantidade_funcionarios AS
SELECT projeto, COUNT(*) AS quantidade_funcionarios
FROM projetos
GROUP BY projeto
ORDER BY quantidade_funcionarios DESC;

-- Visão para a lista de projetos, departamentos e gerentes
CREATE VIEW visao_projetos_departamentos_gerentes AS
SELECT projeto, departamento, gerente
FROM projetos
INNER JOIN departamentos ON projetos.departamento_id = departamentos.id;

-- Visão para os funcionários que possuem dependentes e se são gerentes
CREATE VIEW visao_funcionarios_dependentes_gerentes AS
SELECT empregados.nome, COUNT(dependentes.id) AS quantidade_dependentes, IF(empregados.id = departamentos.gerente_id, 'Sim', 'Não') AS gerente
FROM empregados
LEFT JOIN dependentes ON empregados.id = dependentes.empregado_id
LEFT JOIN departamentos ON empregados.id = departamentos.gerente_id
GROUP BY empregados.nome;

-- Criação de um usuário gerente com permissões de acesso
CREATE USER 'usuario_gerente'@'localhost' IDENTIFIED BY 'senha_segura';
GRANT SELECT ON company.empregados TO 'usuario_gerente'@'localhost';
GRANT SELECT ON company.departamentos TO 'usuario_gerente'@'localhost';
