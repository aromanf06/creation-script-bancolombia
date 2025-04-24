# Consultas SQL para Base de Datos Bancaria

## Enunciado 1: Clientes con múltiples cuentas y sus saldos totales

**Necesidad:** El banco necesita identificar a los clientes que tienen más de una cuenta, mostrar cuántas cuentas tiene cada uno y el saldo total acumulado en todas sus cuentas, ordenados por saldo total de mayor a menor.

**Consulta SQL:**
```sql
select nombre,count(cliente.id_cliente) as total_cuentas,sum(cuenta.saldo) as saldo
from cliente inner join cuenta on cliente.id_cliente=cuenta.id_cliente
group by cliente.id_cliente
having count(cliente.id_cliente) > 1;

```

## Enunciado 2: Comparativa entre depósitos y retiros por cliente

**Necesidad:** El departamento de análisis financiero necesita comparar los montos totales de depósitos y retiros realizados por cada cliente, para identificar patrones de comportamiento financiero.

**Consulta SQL:**
```sql
select nombre,
sum(case when transaccion.tipo_transaccion = 'deposito' then transaccion.monto else 0 end) as depositos,
sum(case when transaccion.tipo_transaccion = 'retiro' then transaccion.monto else 0 end) as retiros
from cliente inner join cuenta on cliente.id_cliente=cuenta.id_cliente
inner join transaccion on cuenta.num_cuenta=transaccion.num_cuenta
group by cliente.id_cliente
```

## Enunciado 3: Cuentas sin tarjetas asociadas

**Necesidad:** El departamento de tarjetas necesita identificar todas las cuentas que no tienen tarjetas asociadas para ofrecer productos específicos a estos clientes.

**Consulta SQL:**
```sql
select cuenta.num_cuenta
from cuenta left join tarjeta on cuenta.num_cuenta=tarjeta.num_cuenta
where tarjeta.num_cuenta is null
```

## Enunciado 4: Análisis de saldos promedio por tipo de cuenta y comportamiento transaccional

**Necesidad:** La gerencia necesita un análisis comparativo del saldo promedio entre cuentas de ahorro y corriente, pero solo considerando aquellas cuentas que han tenido al menos una transacción en los últimos 30 días.

**Consulta SQL:**
```sql
select (select 
    avg(transaccion.MONTO) 
from 
    cuentaahorro
inner join 
    transaccion on cuentaahorro.num_cuenta = transaccion.num_cuenta
where 
    transaccion.fecha >= now() - interval '30 days') as monto_ahorro,
(select 
    avg(transaccion.MONTO)
from 
    cuentacorriente
inner join 
    transaccion on cuentacorriente.num_cuenta = transaccion.num_cuenta
where 
    transaccion.fecha >= now() - interval '30 days') as monto_corriente   
```

## Enunciado 5: Clientes con transferencias pero sin retiros en cajeros

**Necesidad:** El equipo de marketing digital necesita identificar a los clientes que utilizan transferencias pero no realizan retiros por cajeros automáticos, para dirigir campañas de banca digital.

**Consulta SQL:**
```sql

```
