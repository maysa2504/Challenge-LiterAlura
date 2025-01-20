# Challenge-LiterAlura
Challenge JAVA

public class App {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        BookService service = new BookService();
        Database db = new Database();

        while (true) {
            System.out.println("1. Buscar livros na API");
            System.out.println("2. Listar livros no banco");
            System.out.println("3. Filtrar livros por autor");
            System.out.println("4. Remover livro");
            System.out.println("5. Sair");
            System.out.print("Escolha uma opção: ");

            int opcao = scanner.nextInt();
            scanner.nextLine(); // Consumir quebra de linha

            switch (opcao) {
                case 1 -> {
                    try {
                        List<Book> books = service.fetchBooks();
                        for (Book book : books) {
                            db.saveBook(book);
                        }
                        System.out.println("Livros salvos no banco!");
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                case 2 -> {
                    List<Book> books = db.getBooks();
                    books.forEach(b -> System.out.println(b.getTitle() + " - " + b.getAuthors()));
                }
                case 3 -> {
                    System.out.print("Digite o nome do autor: ");
                    String author = scanner.nextLine();
                    List<Book> books = db.getBooks();
                    books.stream()
                         .filter(b -> b.getAuthors().contains(author))
                         .forEach(b -> System.out.println(b.getTitle()));
                }
                case 4 -> {
                    System.out.print("Digite o título do livro para remover: ");
                    String title = scanner.nextLine();
                    db.removeBook(title);
                    System.out.println("Livro removido.");
                }
                case 5 -> {
                    System.out.println("Saindo...");
                    return;
                }
                default -> System.out.println("Opção inválida!");
            }
        }
    }
}
