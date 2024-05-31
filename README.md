# transaccion_postgresql
demo de base de datos con transacciones simple en postgresql

--- sugerencia de CHATgpt.com
-- Conéctate a la base de datos primero si estás usando psql

-- Inicia una transacción
BEGIN;

-- Elimina la columna
ALTER TABLE employees
DROP COLUMN birthdate;

-- Confirma los cambios
COMMIT;

-- En caso de error, revertir los cambios
-- ROLLBACK;


para nuesto demo usamos lo siguiente
--database
create database demo;
---tabla
-- public.users definition

-- Drop table

-- DROP TABLE public.users;

CREATE TABLE public.users (
	id bigserial NOT NULL,
	"name" varchar(255) NOT NULL,
	email varchar(255) NOT NULL,
	email_verified_at timestamp(0) NULL,
	"password" varchar(255) NOT NULL,
	remember_token varchar(100) NULL,
	created_at timestamp(0) NULL,
	updated_at timestamp(0) NULL,
	"role" varchar(255) DEFAULT '1'::character varying NOT NULL,
	CONSTRAINT users_email_unique UNIQUE (email),
	CONSTRAINT users_pkey PRIMARY KEY (id)
);
--commands
SELECT id, "name", email "password" FROM public.users;

BEGIN;

-- Elimina la columna
ALTER TABLE public.users DROP COLUMN email_verified_at;
ALTER TABLE public.users DROP COLUMN remember_token;
ALTER TABLE public.users DROP COLUMN created_at;
ALTER TABLE public.users DROP COLUMN updated_at;
ALTER TABLE public.users DROP COLUMN role;

--regresamos cambios
rollback;
-- Confirma los cambios
COMMIT;

Ejecución en Herramientas GUI
Si estás usando una herramienta GUI como DBeaver, puedes ejecutar estos comandos en el editor de consultas SQL proporcionado por la herramienta.


-- DROP FUNCTION public.validauser(varchar, varchar, varchar);

CREATE OR REPLACE FUNCTION public.validauser(us character varying, pass character varying, em character varying)
 RETURNS TABLE(idx bigint, namex character varying, emailxx character varying, passwordxx character varying)
 LANGUAGE plpgsql
AS $function$
BEGIN
    RETURN QUERY SELECT id, "name", "email", "password" FROM users WHERE email = em  and name = us ;
END;
$function$
;


CREATE OR REPLACE FUNCTION public.validauser(us character varying, pass character varying,em character varying)
  RETURNS TABLE(idx bigint, namex character varying, emailxx character varying, passwordxx character varying) AS
$BODY$
BEGIN
    --RETURN QUERY SELECT id, "name", "email", "password" FROM users WHERE email = em  and name = us ;
	RETURN QUERY SELECT id, "name", "email", "password" FROM users WHERE email = em  ;
END;
$BODY$
  LANGUAGE plpgsql VOLATILE
  COST 100
  ROWS 1000;
 
 
ALTER FUNCTION public.avgigecem(character varying, character varying)
  OWNER TO postgres;

--json
select public.validauser('abrahgam','$2b$10$doX63oF1EW8sMYctlUjB3u4TcIQ2Xv6YYDqYC/kTzSdjh5zpdj3Cq','creors@gmail.com')

--table
select * from  public.validauser('abrahgam','$2b$10$doX63oF1EW8sMYctlUjB3u4TcIQ2Xv6YYDqYC/kTzSdjh5zpdj3Cq','creors@gmail.com')

