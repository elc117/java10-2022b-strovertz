### Atividade 10 - Gleison Pires 

## Explicação Programação Concorrente - Java
A concorrencia: Elemento de um software onde não existe a necessidade de uma execução seqquencial, sua execução pode ser realizada antes ou após a execução de outros dos elementos contidos no software

## Quando vou utilizar o o Synchronized? Tenho dificuldade para entender em que casos devemos utiliza-lo 
Oi,

O synchronized pode ser utilizado sempre que você estiver em um ambiente multi-thread mas tem um trecho de código ou método específico que precisa ser thread-safe, ou seja, onde duas threads mexendo no mesmo objeto podem causar alguma inconsistência.

Por exemplo, imagine que você tenha uma classeConta com a seguinte implementação e que as operações sejam feitas utilizando threads:

```
1 public class Conta {
2    private double saldo;
3
4    public void saca(double valor) {
5        double valorCalculado = saldoAtual - valor;
6        this.saldo = valorCalculado;
7    }
8
9    public void deposita(double valor) {
10        double valorCalculado = saldoAtual + valor;
11        this.saldo = valorCalculado;
12    }
13
```
Caso ocorram uma operação de saque e uma de depósito ao mesmo tempo, e o seguinte cenário:

Conta possui saldo de 1000 reais
Thread1 querendo sacar 200 reais
Thread2 querendo depositar 500 reais
Podemos ter a seguinte situação:

1) Thread1 começa sendo executada então o método saca é chamado;

2) É executada a linha 5. A variável valorCalculado passa a valer 800;

3) Thread1 é pausada para Thread2 poder ser executada então o método deposita é chamado;

4) É executada a linha 10. A variável valorCalculado passa a valer 1500 pois o saldo da Conta ainda não foi atualizado pela Thread1;

5) É executada a linha 11 e o saldo da Conta passa a valer 1500;

6) Thread2 termina e a Thread1 volta a ser executada. A variável valorCalculado está com o valor de 800 que foi calculado anteriormente;

7) É executada a linha 6 e o saldo da Conta passa a valer 800;

8) Thread 1 termina.

Neste cenário perdemos o depósito de 500 reais e para que isto não aconteça, temos que fazer com que aqueles trechos de código sejam síncronos, impedindo a alteração simultânea por várias threads. Fazemos isto com o synchronized. A classe ficaria assim:

```
1 public class Conta {
2    private double saldo;
3
4    public void saca(double valor) {
5        synchronized(this) {
6            double valorCalculado = saldoAtual - valor;
7            this.saldo = valorCalculado;
8        }
9    }
10
11    public void deposita(double valor) {
12        synchronized(this) {
13            double valorCalculado = saldoAtual + valor;
14            this.saldo = valorCalculado;
15        }
16    }
17
```
Ou ainda se quisermos que o método todo seja síncrono e não apenas um trecho específico podemos fazer:

```
1 public class Conta {
2    private double saldo;
3
4    public synchronized void saca(double valor) {
5        double valorCalculado = saldoAtual - valor;
6        this.saldo = valorCalculado;
7    }
8
9    public synchronized void deposita(double valor) {
10        double valorCalculado = saldoAtual + valor;
11        this.saldo = valorCalculado;
12    }
13
```
Pronto! Agora não corremos mais o risco de ter inconsistências nesse objeto.

Ficou mais claro? Qualquer coisa só falar!

Abraço!

