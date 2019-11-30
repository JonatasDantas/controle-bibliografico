# controle-bibliografico
Aplicação Java para Controle Bibliográfico - EP Banco de Dados USP



queries:
`
CREATE TABLE public.publicacoes (
	id SERIAL,
	tipo_publicacao VARCHAR(20) NOT NULL,
	titulo VARCHAR(80),
	cidade VARCHAR(60),
	estado VARCHAR(40),
	pais VARCHAR(40),
	CONSTRAINT publicacoes_pkey PRIMARY KEY (id)
);

CREATE TABLE public.artigos (
	id SERIAL UNIQUE,
	id_publicacao INTEGER NOT NULL,
	tipo_artigo VARCHAR(40) NOT NULL,
	editora VARCHAR,
	CONSTRAINT artigos_pkey PRIMARY KEY (id, id_publicacao),
	FOREIGN KEY (id_publicacao) REFERENCES public.publicacoes (id)
);

CREATE TABLE public.artigos_de_periodicos (
	id SERIAL,
	numero_volume INTEGER NOT NULL,
	id_artigo INTEGER NOT NULL,
	pagina_inicial INTEGER,
	pagina_final INTEGER,
	data_mes INTEGER,
	data_ano INTEGER,
	CONSTRAINT artigos_de_periodicos_pkey PRIMARY KEY (id, id_artigo),
	FOREIGN KEY (id_artigo) REFERENCES public.artigos (id)
);

CREATE TABLE public.artigos_de_livros (
	id SERIAL,
	numero_da_edicao INTEGER NOT NULL,
	id_artigo INTEGER NOT NULL,
	titulo_original VARCHAR(80),
	pagina_inicial INTEGER,
	pagina_final INTEGER,
	num_capitulo INTEGER,
	ano_publicacao INTEGER,
	CONSTRAINT artigos_de_livros_pkey PRIMARY KEY (id, id_artigo),
	FOREIGN KEY (id_artigo) REFERENCES public.artigos (id)
);

CREATE TABLE public.artigos_de_anais (
	id SERIAL,
	id_artigo INTEGER NOT NULL,
	num_indicacao INTEGER NOT NULL,
	vol_indicacao VARCHAR,
	data_mes_indicacao INTEGER,
	data_ano_indicacao INTEGER,
	pagina_inicial INTEGER,
	pagina_final INTEGER,
	CONSTRAINT artigos_de_anais_pkey PRIMARY KEY (id, id_artigo),
	FOREIGN KEY (id_artigo) REFERENCES public.artigos (id)
);

CREATE TABLE public.autores (
	id SERIAL UNIQUE,
	nome_autor VARCHAR NOT NULL,
	CONSTRAINT autores_pkey PRIMARY KEY (id)
);

ALTER TABLE public.artigos_de_anais ADD UNIQUE (id);
ALTER TABLE public.artigos_de_livros ADD UNIQUE (id);
ALTER TABLE public.artigos_de_periodicos ADD UNIQUE (id);

CREATE TABLE public.periodicos (
	id SERIAL UNIQUE,
	id_publicacao INTEGER NOT NULL,
	editora VARCHAR,
	data_prim_volume date,
	data_ult_volume date,
	CONSTRAINT periodicos_pkey PRIMARY KEY (id, id_publicacao),
	FOREIGN KEY (id_publicacao) REFERENCES public.publicacoes (id)
);

CREATE TABLE public.periodicos_artigosDePeriodicos (
	id_periodico INTEGER NOT NULL,
	id_artig_period INTEGER NOT NULL,
	CONSTRAINT periodicos_artigosDePeriodicos_pkey PRIMARY KEY (id_periodico, id_artig_period),
	FOREIGN KEY (id_periodico) REFERENCES public.periodicos (id),
	FOREIGN KEY (id_artig_period) REFERENCES public.artigos_de_periodicos (id)
);

CREATE TABLE public.monografias (
	id SERIAL UNIQUE,
	id_publicacao INTEGER NOT NULL,
	nome_instituição VARCHAR,
	data_mes INTEGER,
	data_ano INTEGER,
	CONSTRAINT monografias_pkey PRIMARY KEY (id, id_publicacao),
	FOREIGN KEY (id_publicacao) REFERENCES public.publicacoes (id)
);


CREATE TABLE public.anais_de_conferencias (
	id SERIAL UNIQUE,
	id_publicacao INTEGER NOT NULL,
	editora VARCHAR,
	indicacao_volume INTEGER,
	indicacao_num INTEGER,
	indicacao_data_mes INTEGER,
	indicacao_data_ano INTEGER,
	CONSTRAINT anais_de_conferencias_pkey PRIMARY KEY (id, id_publicacao),
	FOREIGN KEY (id_publicacao) REFERENCES public.publicacoes (id)
);

CREATE TABLE public.anaisConf_artigosDeAnais (
	id_anal_conf INTEGER NOT NULL,
	id_artig_anal INTEGER NOT NULL,
	CONSTRAINT anaisConf_artigosDeAnais_pkey PRIMARY KEY (id_anal_conf, id_artig_anal),
	FOREIGN KEY (id_anal_conf) REFERENCES public.anais_de_conferencias (id),
	FOREIGN KEY (id_artig_anal) REFERENCES public.artigos_de_anais (id)
);

CREATE TABLE public.livros (
	id SERIAL UNIQUE,
	id_publicacao INTEGER NOT NULL,
	tipo_livro VARCHAR NOT NULL,
	titulo_original VARCHAR NOT NULL,
	num_paginas INTEGER,
	editora VARCHAR,
	num_edicao INTEGER,
	ano_public INTEGER,
	CONSTRAINT livros_pkey PRIMARY KEY (id, id_publicacao),
	FOREIGN KEY (id_publicacao) REFERENCES public.publicacoes (id)
);

CREATE TABLE public.livros_artigos (
	id SERIAL UNIQUE,
	id_livro INTEGER NOT NULL,
	CONSTRAINT livros_artigos_pkey PRIMARY KEY (id, id_livro),
	FOREIGN KEY (id_livro) REFERENCES public.livros (id)
);

CREATE TABLE public.livrosArtigos_artigosDeLivros (
	id_livro_de_artigo INTEGER NOT NULL,
	id_artig_livro INTEGER NOT NULL,
	CONSTRAINT livrosArtigos_artigosDeLivros_pkey PRIMARY KEY (id_livro_de_artigo, id_artig_livro),
	FOREIGN KEY (id_livro_de_artigo) REFERENCES public.livros_artigos (id),
	FOREIGN KEY (id_artig_livro) REFERENCES public.artigos_de_livros (id)
);


CREATE TABLE public.livros_capitulos (
	id SERIAL UNIQUE,
	id_livro INTEGER NOT NULL,
	CONSTRAINT livros_capitulos_pkey PRIMARY KEY (id, id_livro),
	FOREIGN KEY (id_livro) REFERENCES public.livros (id)
);

CREATE TABLE public.capitulos (
	id SERIAL,
	id_livro_capitulos INTEGER NOT NULL,
	CONSTRAINT capitulos_pkey PRIMARY KEY (id, id_livro_capitulos),
	FOREIGN KEY (id_livro_capitulos) REFERENCES public.livros_capitulos (id)
);

CREATE TABLE public.publicacoes_autores (
	id_publicacao INTEGER NOT NULL,
	id_autor INTEGER NOT NULL,
	CONSTRAINT publicacoes_autores_pkey PRIMARY KEY (id_publicacao, id_autor),
	FOREIGN KEY (id_publicacao) REFERENCES public.publicacoes (id),
	FOREIGN KEY (id_autor) REFERENCES public.autores (id)
);
`
