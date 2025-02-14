# SchoolStack
App para desenvolvimento de alunos de TI. 
Descrição: App ou plataforma de ensino que permite inscrever os alunos por classe, pode ser permitido que todos os professores coloquem conteúdo em função da sua disciplina, gerar o plano mensal e diário. Os alunos podem acompanhar a matéria, fazer exercícios e tarefas e as mesmas marcam o seu progresso. As avaliações podem ser feitas a partir da plataforma que integra um IDE para exercícios de desenvolvimento prático e um simulador de redes, reparação de computadores, office e outros. Parecido a ideia do Google Classroom.
Permitir que escolas se cadastrem para que seus alunos possam fazer parte dos alunos com privilêgios. É possível entusiastas e pessoas que queiram aprimorar também façam inscrição.
A ideia também passa por adicionar empresas que notificam as vagas de estágio. (estagio.ao)

# 1. Planejamento Inicial
## 1.1. Definição do Escopo
Ensino de disciplinas-chave + cursos extras.
Cadastro de escolas com privilégios exclusivos para alunos.
Empresas cadastrando vagas de estágio e desafios.
Professores acompanhando alunos e contribuindo com conteúdo.
Avaliações e progresso acadêmico integrado.
Integração com o calendário escolar.
## 1.2. Estudo de Mercado
Analisar plataformas como Luanda42, Dio.me, Rocketseat e Alura.
Entender desafios e oportunidades no setor educacional local.
Identificar requisitos legais para possível adoção pelo Ministério da Educação.
## 1.3. Tecnologias Escolhidas
Back-end: Laravel (API Restful)
Banco de Dados: PostgreSQL
Front-end: React.js ou Blade
Autenticação: Laravel Sanctum ou Passport
Armazenamento: AWS S3 ou DigitalOcean Spaces (para vídeos e arquivos)
Mensageria: WebSockets ou Firebase para notificações em tempo real
# 2. Fases do Desenvolvimento
## 2.1. Modelagem do Banco de Dados
Entidades Principais
Usuários (alunos, professores, administradores, empresas)
Escolas (com alunos e professores associados)
Cursos (com módulos e aulas)
Aulas (com vídeos, textos e quizzes)
Matrículas (vinculação aluno-curso)
Roadmaps e Planos de Aula (para professores)
Empresas e Vagas (para estágios e desafios)
Calendário Escolar (com eventos e prazos)
## 2.2. Desenvolvimento do MVP (Produto Mínimo Viável)
### Fase 1: Autenticação e Cadastro
Login e cadastro com diferentes tipos de usuários.
Permitir que escolas e empresas se cadastrem.
Sistema de permissões e privilégios.
### Fase 2: Gestão de Cursos
Criar estrutura de cursos, módulos e aulas.
Upload de materiais e vídeos.
Quiz e exercícios interativos.
### Fase 3: Acompanhamento e Avaliação
Progresso dos alunos em tempo real.
Professores adicionando feedback.
Certificados automáticos ao completar cursos.
### Fase 4: Integração com Empresas
Empresas postando vagas de estágio e desafios.
Sistema de recomendação de alunos com base nas habilidades adquiridas.
### Fase 5: Integração com o Calendário Escolar
Eventos acadêmicos e prazos sincronizados.
Notificações para alunos e professores.
### Fase 6: Expansão e Suporte
Otimização do sistema e segurança.
Implementação de novas funcionalidades com base no feedback.
# 3. Modelagem da Estrutura
## 3.1. Arquitetura
Backend: API Restful com Laravel.
Frontend: React.js para interface dinâmica ou Blade para maior integração com Laravel.
Banco de Dados: PostgreSQL estruturado para consultas rápidas.
## 3.2. Estrutura de Diretórios (Laravel)
//Apresentar
# 4. Funcionalidades Adicionais
Gamificação: Recompensas, desafios e ranking de alunos.
IA para Sugestões: Indicar cursos e desafios personalizados.
Mobile App: Criar versão para iOS/Android futuramente.
# 5. Sugestões e Próximos Passos
Criar wireframes e mockups da interface.
Desenvolver protótipos funcionais antes do código final.
Testar com um grupo piloto antes do lançamento.
Planejar parcerias estratégicas com escolas e empresas.
Construir um plano de monetização (assinaturas, cursos pagos, parcerias).

# 6. Estrutura do Banco de Dados (PostgreSQL)
A modelagem segue um formato relacional, garantindo escalabilidade e eficiência.

## 6.1. Tabelas Principais
1. Usuários (users)
Guarda as informações de alunos, professores, administradores e empresas.

CREATE TABLE users (

    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role ENUM('admin', 'aluno', 'professor', 'empresa') NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
2. Escolas (schools)
Escolas podem se cadastrar para oferecer privilégios aos alunos.

CREATE TABLE schools (

    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
3. Cursos (courses)
Contém os cursos disponíveis na plataforma.

CREATE TABLE courses (

    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    created_by INT REFERENCES users(id) ON DELETE SET NULL,
    school_id INT REFERENCES schools(id) ON DELETE SET NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
4. Módulos do Curso (modules)
Cada curso é dividido em módulos.

CREATE TABLE modules (

    id SERIAL PRIMARY KEY,
    course_id INT REFERENCES courses(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    position INT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
5. Aulas (lessons)
Cada módulo possui várias aulas, que podem conter vídeos, artigos e exercícios.

CREATE TABLE lessons (

    id SERIAL PRIMARY KEY,
    module_id INT REFERENCES modules(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    content TEXT,
    video_url VARCHAR(255),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
6. Matrículas (enrollments)
Alunos se matriculam em cursos.

CREATE TABLE enrollments (

    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    course_id INT REFERENCES courses(id) ON DELETE CASCADE,
    status ENUM('em andamento', 'concluído', 'cancelado') DEFAULT 'em andamento',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
7. Progresso do Aluno (progress)
Monitora o avanço do aluno nas aulas.

CREATE TABLE progress (

    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    lesson_id INT REFERENCES lessons(id) ON DELETE CASCADE,
    status ENUM('não iniciado', 'em andamento', 'concluído') DEFAULT 'não iniciado',
    last_accessed TIMESTAMP DEFAULT NOW()
);
2. Funcionalidades Adicionais
2.1. Sistema de Avaliações
Avaliações (assessments)

CREATE TABLE assessments (

    id SERIAL PRIMARY KEY,
    course_id INT REFERENCES courses(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
Questões das Avaliações (questions)

CREATE TABLE questions (

    id SERIAL PRIMARY KEY,
    assessment_id INT REFERENCES assessments(id) ON DELETE CASCADE,
    question TEXT NOT NULL,
    question_type ENUM('multiple_choice', 'open_ended') NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
Respostas Possíveis (options)

CREATE TABLE options (

    id SERIAL PRIMARY KEY,
    question_id INT REFERENCES questions(id) ON DELETE CASCADE,
    option_text TEXT NOT NULL,
    is_correct BOOLEAN DEFAULT FALSE
);
Respostas dos Alunos (answers)

CREATE TABLE answers (

    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    question_id INT REFERENCES questions(id) ON DELETE CASCADE,
    selected_option_id INT REFERENCES options(id) ON DELETE SET NULL,
    open_answer TEXT
);
3. Integração com Empresas
Empresas (companies)

CREATE TABLE companies (

    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
Vagas de Estágio (job_offers)

CREATE TABLE job_offers (

    id SERIAL PRIMARY KEY,
    company_id INT REFERENCES companies(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    requirements TEXT,
    salary_range VARCHAR(50),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
Aplicações dos Alunos para Estágios (job_applications)

CREATE TABLE job_applications (

    id SERIAL PRIMARY KEY,
    job_offer_id INT REFERENCES job_offers(id) ON DELETE CASCADE,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    status ENUM('pendente', 'aceito', 'rejeitado') DEFAULT 'pendente',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
4. Integração com o Calendário Escolar
Calendário (calendar_events)

CREATE TABLE calendar_events (

    id SERIAL PRIMARY KEY,
    school_id INT REFERENCES schools(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    event_date TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
