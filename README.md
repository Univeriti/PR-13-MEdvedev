import java.util.Scanner;
import java.text.SimpleDateFormat;
import java.util.Date;

class DATA {
    private static final int MAX_RECORDS = 50;
    private static String[] dates = new String[MAX_RECORDS];
    private static String[] entries = new String[MAX_RECORDS];
    private static int currentIndex = 0;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nМеню:");
            System.out.println("1. Додати запис");
            System.out.println("2. Видалити запис за датою");
            System.out.println("3. Переглянути всі записи");
            System.out.println("4. Вихід");
            System.out.print("Виберіть опцію: ");

            int option = scanner.nextInt();
            scanner.nextLine();

            switch (option) {
                case 1:
                    addEntry(scanner);
                    break;
                case 2:
                    deleteEntry(scanner);
                    break;
                case 3:
                    viewEntries();
                    break;
                case 4:
                    System.out.println("Вихід з програми.");
                    scanner.close();
                    return;
                default:
                    System.out.println("Невірний вибір. Спробуйте ще раз.");
            }
        }
    }

    private static void addEntry(Scanner scanner) {
        if (currentIndex >= MAX_RECORDS) {
            System.out.println("Щоденник заповнений, не можна додавати більше записів.");
            return;
        }

        System.out.print("Введіть дату (формат: dd.MM.yyyy): ");
        String date = scanner.nextLine();

        if (!isValidDate(date)) {
            System.out.println("Некоректний формат дати.");
            return;
        }

        System.out.println("Введіть ваш запис:");
        StringBuilder entry = new StringBuilder();
        String line;
        while (true) {
            line = scanner.nextLine();
            if (line.isEmpty()) break;
            entry.append(line).append("\n");
        }

        dates[currentIndex] = date;
        entries[currentIndex] = entry.toString().trim();
        currentIndex++;
        System.out.println("Запис додано!");
    }

    private static void deleteEntry(Scanner scanner) {
        System.out.print("Введіть дату запису, який хочете видалити (формат: dd.MM.yyyy): ");
        String date = scanner.nextLine();

        int indexToDelete = findRecordIndexByDate(date);

        if (indexToDelete == -1) {
            System.out.println("Запис з такою датою не знайдений.");
            return;
        }

        for (int i = indexToDelete; i < currentIndex - 1; i++) {
            dates[i] = dates[i + 1];
            entries[i] = entries[i + 1];
        }

        dates[currentIndex - 1] = null;
        entries[currentIndex - 1] = null;
        currentIndex--;
        System.out.println("Запис видалено.");
    }

    private static void viewEntries() {
        if (currentIndex == 0) {
            System.out.println("Немає записів для перегляду.");
            return;
        }

        for (int i = 0; i < currentIndex; i++) {
            System.out.println("Дата: " + dates[i]);
            System.out.println("Запис: " + entries[i]);
            System.out.println("-------------------------");
        }
    }

    private static boolean isValidDate(String date) {
        SimpleDateFormat sdf = new SimpleDateFormat("dd.MM.yyyy");
        sdf.setLenient(false);
        try {
            Date parsedDate = sdf.parse(date);
            return true;
        } catch (Exception e) {
            return false;
        }
    }

    private static int findRecordIndexByDate(String date) {
        for (int i = 0; i < currentIndex; i++) {
            if (dates[i].equals(date)) {
                return i;
            }
        }
        return -1;
    }
}
