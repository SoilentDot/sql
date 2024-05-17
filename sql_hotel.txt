CREATE TABLE public.hotel
(
    "hotelNo" integer,
    "hotelName" character varying(30) NOT NULL,
    city character varying(25) NOT NULL,
    address character varying(35) NOT NULL,
    PRIMARY KEY ("hotelNo")
);

ALTER TABLE IF EXISTS public.hotel
    OWNER to postgres;

CREATE TABLE public.room
(
    "roomNo" integer NOT NULL,
    "hotelNo" integer,
    type character varying(6) NOT NULL,
    price numeric NOT NULL,
    PRIMARY KEY ("roomNo"),
    CONSTRAINT "hotelNo_fk" FOREIGN KEY ("hotelNo")
        REFERENCES public.hotel ("hotelNo") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT type CHECK ("type" IN ('Single', 'Double', 'Suite')) NOT VALID,
    CONSTRAINT price CHECK (price >= 200.00 AND price <= 2000.00) NOT VALID,
    CONSTRAINT "roomNo" CHECK ("roomNo" >= 100 AND "roomNo" <= 400) NOT VALID
);

ALTER TABLE IF EXISTS public.room
    OWNER to postgres;

CREATE TABLE public.guest
(
    "guestNo" integer,
    "guestName" character varying(30) NOT NULL,
    "guestAddress" character varying(35) NOT NULL,
    pone character varying(10) NOT NULL,
    PRIMARY KEY ("guestNo")
);

ALTER TABLE IF EXISTS public.guest
    OWNER to postgres;

CREATE TABLE public.booking
(
    "hotelNo" integer,
    "guestNo" integer,
    "dateFrom" date NOT NULL,
    "dateTo" date NOT NULL,
    "roomNo" integer,
    CONSTRAINT "hotelNo_fk" FOREIGN KEY ("hotelNo")
        REFERENCES public.hotel ("hotelNo") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT "guestNo_fk" FOREIGN KEY ("guestNo")
        REFERENCES public.guest ("guestNo") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT "roomNo_fk" FOREIGN KEY ("roomNo")
        REFERENCES public.room ("roomNo") MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT "dateFrom" CHECK ("dateFrom" > current_date) NOT VALID,
    CONSTRAINT "dateTo" CHECK ("dateTo" > current_date) NOT VALID
);

ALTER TABLE IF EXISTS public.booking
    OWNER to postgres;