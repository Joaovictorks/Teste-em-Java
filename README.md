# Teste-em-Java
import java.time.LocalDate;

public class Pessoa {
    private String nome;
    private LocalDate dataNascimento;

    public Pessoa(String nome, LocalDate dataNascimento) {
        this.nome = nome;
        this.dataNascimento = dataNascimento;
    }

    public String getNome() {
        return nome;
    }

    public LocalDate getDataNascimento() {
        return dataNascimento;
    }
}
import java.math.BigDecimal;
import java.time.LocalDate;

public class Funcionario extends Pessoa {
    private BigDecimal salario;
    private String funcao;

    public Funcionario(String nome, LocalDate dataNascimento, BigDecimal salario, String funcao) {
        super(nome, dataNascimento);
        this.salario = salario;
        this.funcao = funcao;
    }

    public BigDecimal getSalario() {
        return salario;
    }

    public void setSalario(BigDecimal salario) {
        this.salario = salario;
    }

    public String getFuncao() {
        return funcao;
    }
}
import java.math.BigDecimal;
import java.text.NumberFormat;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.*;
import java.util.stream.Collectors;

public class Principal {

    public static void main(String[] args) {
        List<Funcionario> funcionarios = new ArrayList<>();

        funcionarios.add(new Funcionario("Maria", LocalDate.of(2000, 10, 18), new BigDecimal("2009.44"), "Operador"));
        funcionarios.add(new Funcionario("João", LocalDate.of(1990, 5, 12), new BigDecimal("2284.38"), "Operador"));
        funcionarios.add(new Funcionario("Caio", LocalDate.of(1961, 5, 2), new BigDecimal("9836.14"), "Coordenador"));
        funcionarios.add(new Funcionario("Miguel", LocalDate.of(1988, 10, 14), new BigDecimal("19119.88"), "Diretor"));
        funcionarios.add(new Funcionario("Alice", LocalDate.of(1995, 1, 5), new BigDecimal("2234.68"), "Recepcionista"));
        funcionarios.add(new Funcionario("Heitor", LocalDate.of(1999, 11, 19), new BigDecimal("1582.72"), "Operador"));
        funcionarios.add(new Funcionario("Arthur", LocalDate.of(1993, 3, 31), new BigDecimal("4071.84"), "Contador"));
        funcionarios.add(new Funcionario("Laura", LocalDate.of(1994, 7, 8), new BigDecimal("3017.45"), "Gerente"));
        funcionarios.add(new Funcionario("Heloísa", LocalDate.of(2003, 5, 24), new BigDecimal("1606.85"), "Eletricista"));
        funcionarios.add(new Funcionario("Helena", LocalDate.of(1996, 9, 2), new BigDecimal("2799.93"), "Gerente"));

        // 3.2 – Remover o funcionário “João” da lista.
        funcionarios.removeIf(funcionario -> funcionario.getNome().equals("João"));

        // Formatadores para data e números
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
        NumberFormat nf = NumberFormat.getInstance(new Locale("pt", "BR"));

        // 3.3 – Imprimir todos os funcionários
        System.out.println("3.3 – Lista de Funcionários:");
        for (Funcionario funcionario : funcionarios) {
            System.out.println("Nome: " + funcionario.getNome() +
                    ", Data de Nascimento: " + funcionario.getDataNascimento().format(formatter) +
                    ", Salário: " + nf.format(funcionario.getSalario()) +
                    ", Função: " + funcionario.getFuncao());
        }

        // 3.4 – Aumento de 10% no salário
        for (Funcionario funcionario : funcionarios) {
            BigDecimal novoSalario = funcionario.getSalario().multiply(new BigDecimal("1.10"));
            funcionario.setSalario(novoSalario);
        }

        // 3.5 – Agrupar os funcionários por função em um MAP
        Map<String, List<Funcionario>> funcionariosPorFuncao = funcionarios.stream()
                .collect(Collectors.groupingBy(Funcionario::getFuncao));

        // 3.6 – Imprimir os funcionários agrupados por função
        System.out.println("3.6 – Funcionários agrupados por função:");
        for (Map.Entry<String, List<Funcionario>> entry : funcionariosPorFuncao.entrySet()) {
            System.out.println("Função: " + entry.getKey());
            for (Funcionario funcionario : entry.getValue()) {
                System.out.println("\tNome: " + funcionario.getNome());
            }
        }

        // 3.8 – Funcionários que fazem aniversário em outubro e dezembro
        System.out.println("3.8 – Funcionários que fazem aniversário em outubro e dezembro:");
        for (Funcionario funcionario : funcionarios) {
            int mesAniversario = funcionario.getDataNascimento().getMonthValue();
            if (mesAniversario == 10 || mesAniversario == 12) {
                System.out.println(funcionario.getNome());
            }
        }

        // 3.9 – Funcionário com a maior idade
        Funcionario funcionarioMaisVelho = Collections.min(funcionarios, Comparator.comparing(Funcionario::getDataNascimento));
        int idadeMaisVelho = LocalDate.now().getYear() - funcionarioMaisVelho.getDataNascimento().getYear();
        System.out.println("3.9 – Funcionário com a maior idade: " + funcionarioMaisVelho.getNome() + ", Idade: " + idadeMaisVelho);

        // 3.10 – Funcionários em ordem alfabética
        List<Funcionario> funcionariosOrdenados = new ArrayList<>(funcionarios);
        funcionariosOrdenados.sort(Comparator.comparing(Funcionario::getNome));
        System.out.println("3.10 – Funcionários em ordem alfabética:");
        for (Funcionario funcionario : funcionariosOrdenados) {
            System.out.println(funcionario.getNome());
        }

        // 3.11 – Total dos salários
        BigDecimal totalSalarios = funcionarios.stream()
                .map(Funcionario::getSalario)
                .reduce(BigDecimal.ZERO, BigDecimal::add);
        System.out.println("3.11 – Total dos salários: " + nf.format(totalSalarios));

        // 3.12 – Salários em relação ao salário mínimo
        BigDecimal salarioMinimo = new BigDecimal("1212.00");
        System.out.println("3.12 – Salários em relação ao salário mínimo:");
        for (Funcionario funcionario : funcionarios) {
            BigDecimal salariosMinimos = funcionario.getSalario().divide(salarioMinimo, 2, BigDecimal.ROUND_HALF_UP);
            System.out.println(funcionario.getNome() + ": " + salariosMinimos + " salários mínimos");
        }
    }
}

