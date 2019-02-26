# Practice

Какие стримы есть в стандартной библиотеке?

 - LongStream (+)
 - StringStream
 - Stream (+)
 - IntStream (+)
 - DoubleStream (+)
 - FloatStream
 - ByteStream
 - CharStream
 
Выберите верные утверждения.

 - Элементами стрима могут быть только объекты, стримы примитивов не разрешены.
 - Стрим позволяет применять любое количество промежуточных операций. Все они запустятся только тогда, когда на стриме будет вызвана терминальная операция. (+)
 - Стримы "одноразовые", т.е. после вызова терминальной операции стрим больше не пригоден к использованию. (+)
 - Стрим надо обязательно всегда закрывать, т.е. вызывать метод его close() или использовать в блоке try с ресурсами.
 - Применение трансформаций к элементам стрима изменяет содержимое источника, откуда стрим берет элементы (например, коллекции или массива).
 
Отметьте галочками промежуточные операции со стримом.

 - flatMap (+)
 - collect
 - sorted (+)
 - count
 - limit (+)
 - map (+)
 - filter (+)
 - toArray
 - peek (+)
 - forEach
 
Задача № 1

Напишите метод, возвращающий стрим псевдослучайных целых чисел. Алгоритм генерации чисел следующий:

Берется какое-то начальное неотрицательное число (оно будет передаваться в ваш метод проверяющей системой).
Первый элемент последовательности устанавливается равным этому числу.
Следующие элементы вычисляются по рекуррентной формуле Rn+1=mid(R2n), где mid — это функция, выделяющая второй, третий и четвертый разряд переданного числа. Например, mid(123456)=345.

Решение:

    public static IntStream pseudoRandomStream(int seed) {
            return IntStream.iterate(seed, n -> {
                        n = n * n;
                        int i = 0, result = 0, ten = 1;
                        while (n > 0) {
                            if (i >= 1) {
                                if (i < 4) {
                                    result += (n % 10) * ten;
                                    ten *= 10;
 
                                }
                            }
                            n /= 10;
                            i++;
                        }
                        return result;
            }
            );
    }
    
Задача № 2

Напишите метод, находящий в стриме минимальный и максимальный элементы в соответствии порядком, заданным Comparator'ом.
Найденные минимальный и максимальный элементы передайте в minMaxConsumer следующим образом:

    minMaxConsumer.accept(min, max);

Если стрим не содержит элементов, то вызовите

    minMaxConsumer.accept(null, null);

Решение:

    public static <T> void findMinMax(
        Stream<? extends T> stream,
        Comparator<? super T> order,
        BiConsumer<? super T, ? super T> minMaxConsumer) {

        final Object[] max = new Object[1];
        final Object[] min = new Object[1];
        max[0] = null;
        min[0] = null;
        stream.forEach(e -> {
            if (max[0] == null) max[0] = e;
            if (min[0] == null) min[0] = e;
            int t = order.compare((T)max[0], e);
            if (t < 0) max[0] = e;
            t = order.compare((T)min[0], e);
            if (t > 0) min[0] = e;
        });
        minMaxConsumer.accept(min[0] != null?(T)min[0]:null, max[0] != null?(T)max[0]:null);
    }

Задача № 3

Напишите программу, читающую из System.in текст в кодировке UTF-8, подсчитывающую в нем частоту появления слов, и в конце выводящую 10 наиболее часто встречающихся слов.
Словом будем считать любую непрерывную последовательность символов, состоящую только из букв и цифр. Например, в строке "Мама мыла раму 33 раза!" ровно пять слов: "Мама", "мыла", "раму", "33" и "раза".
Подсчет слов должен выполняться без учета регистра, т.е. "МАМА", "мама" и "Мама" — это одно и то же слово. Выводите слова в нижнем регистре.
Если в тексте меньше 10 уникальных слов, то выводите сколько есть.
Если в тексте некоторые слова имеют одинаковую частоту, т.е. их нельзя однозначно упорядочить только по частоте, то дополнительно упорядочите слова с одинаковой частотой в лексикографическом порядке.
Задача имеет красивое решение через стримы без циклов и условных операторов. Попробуйте придумать его.

Решение:

    import java.io.IOException;
    import java.util.Comparator;
    import java.util.HashMap;
    import java.util.Map;
    import java.util.Scanner;
    public class Main {
        public static void main(String[] args) throws IOException {
            Scanner scanner = new Scanner(System.in, "UTF-8")
                .useDelimiter("[^\\p{L}\\p{Digit}]+");
            Map<String, Integer> freqMap = new HashMap<>();
            scanner.forEachRemaining(s -> freqMap.merge(s.toLowerCase(), 1, (a, b) -> a + b));
            freqMap.entrySet().stream()
                .sorted(descendingFrequencyOrder())
                .limit(10)
                .map(Map.Entry::getKey)
                .forEach(System.out::println);
        }

        private static Comparator<Map.Entry<String, Integer>> descendingFrequencyOrder() {
            return Comparator.<Map.Entry<String, Integer>>comparingInt(Map.Entry::getValue)
                .reversed()
                .thenComparing(Map.Entry::getKey);
        }
    }
    
Задача № 4

В этой задаче вам предстоит самостоятельно написать набор классов таким образом, чтобы данный код успешно компилировался и выполнялся. 
Вам предстоит использовать новые знания про generics, коллекции и Stream API.
В приведенном коде используется оператор assert. Этот оператор используется для того, чтобы проверять определенные инварианты в коде. С помощью него возможно писать небольшие тесты и следить за корректностью своей программы (в обычной ситуации предпочтительно для этих целей использовать библиотеки для модульного тестирования, которые выходят за рамки базового курса).
Оператор выглядит следующим образом: 
assert предикат: сообщение;
Предикат – выражение с типом boolean. Если выражение является ложным, то в программе возникает исключение AssertionError с соответствующим сообщением.
По-умолчанию данная функциональность отключена. Чтобы ее включить, необходимо передать специальный ключ -ea в параметры виртуальной машины. Сделать это можно прямо при запуске в консоли с помощью программы java, либо указав этот ключ в настройках запуска программы в вашей IDE. В случае IntellijIDEA, например, эта опция указывается в поле Run -> Edit Configurations... -> конфигурация запуска вашей программы -> VM Options.
Код, который необходимо заставить успешно работать:

    // Random variables
    String randomFrom = "..."; // Некоторая случайная строка. Можете выбрать ее самостоятельно. 
    String randomTo = "...";  // Некоторая случайная строка. Можете выбрать ее самостоятельно.
    int randomSalary = 100;  // Некоторое случайное целое положительное число. Можете выбрать его самостоятельно.

    // Создание списка из трех почтовых сообщений.
    MailMessage firstMessage = new MailMessage(
        "Robert Howard",
        "H.P. Lovecraft",
        "This \"The Shadow over Innsmouth\" story is real masterpiece, Howard!"
    );

    assert firstMessage.getFrom().equals("Robert Howard"): "Wrong firstMessage from address";
    assert firstMessage.getTo().equals("H.P. Lovecraft"): "Wrong firstMessage to address";
    assert firstMessage.getContent().endsWith("Howard!"): "Wrong firstMessage content ending";

    MailMessage secondMessage = new MailMessage(
        "Jonathan Nolan",
        "Christopher Nolan",
        "Брат, почему все так хвалят только тебя, когда практически все сценарии написал я. Так не честно!"
    );

    MailMessage thirdMessage = new MailMessage(
        "Stephen Hawking",
        "Christopher Nolan",
        "Я так и не понял Интерстеллар."
    );

    List<MailMessage> messages = Arrays.asList(
        firstMessage, secondMessage, thirdMessage
    );

    // Создание почтового сервиса.
    MailService<String> mailService = new MailService<>();

    // Обработка списка писем почтовым сервисом
    messages.stream().forEachOrdered(mailService);

    // Получение и проверка словаря "почтового ящика",
    //   где по получателю можно получить список сообщений, которые были ему отправлены
    Map<String, List<String>> mailBox = mailService.getMailBox();

    assert mailBox.get("H.P. Lovecraft").equals(
        Arrays.asList(
                "This \"The Shadow over Innsmouth\" story is real masterpiece, Howard!"
        )
    ): "wrong mailService mailbox content (1)";

    assert mailBox.get("Christopher Nolan").equals(
        Arrays.asList(
                "Брат, почему все так хвалят только тебя, когда практически все сценарии написал я. Так не честно!",
                "Я так и не понял Интерстеллар."
        )
    ): "wrong mailService mailbox content (2)";

    assert mailBox.get(randomTo).equals(Collections.<String>emptyList()): "wrong mailService mailbox content (3)";


    // Создание списка из трех зарплат.
    Salary salary1 = new Salary("Facebook", "Mark Zuckerberg", 1);
    Salary salary2 = new Salary("FC Barcelona", "Lionel Messi", Integer.MAX_VALUE);
    Salary salary3 = new Salary(randomFrom, randomTo, randomSalary);

    // Создание почтового сервиса, обрабатывающего зарплаты.
    MailService<Integer> salaryService = new MailService<>();

    // Обработка списка зарплат почтовым сервисом
    Arrays.asList(salary1, salary2, salary3).forEach(salaryService);

    // Получение и проверка словаря "почтового ящика",
    //   где по получателю можно получить список зарплат, которые были ему отправлены.
    Map<String, List<Integer>> salaries = salaryService.getMailBox();
    assert salaries.get(salary1.getTo()).equals(Arrays.asList(1)): "wrong salaries mailbox content (1)";
    assert salaries.get(salary2.getTo()).equals(Arrays.asList(Integer.MAX_VALUE)): "wrong salaries mailbox content (2)";
    assert salaries.get(randomTo).equals(Arrays.asList(randomSalary)): "wrong salaries mailbox content (3)";

Решение:

    public interface IMessage<T> {
        String getFrom();
        String getTo();
        T getContent();
    }
    public static class MailMessage implements IMessage<String>{
        private String from;
        private String to;
        private String content;
    
        public MailMessage(String from, String to, String content){
            this.from = from;
            this.to = to;
            this.content = content;
        }
        public final String getFrom() {
            return from;
        }
        public final String getTo() {
            return to;
        }
        public final String getContent(){
            return content;
        }
    }

    public static class Salary implements IMessage<Integer>{
        private String from;
        private String to;
        private Integer content;

        public Salary(String from, String to, Integer content){
            this.from = from;
            this.to = to;
            this.content = content;
        }
        public final String getFrom() {
            return from;
        }
        public final String getTo() {
            return to;
        }
        public final Integer getContent(){
            return content;
        }
    }

    public static class MailService<T> implements Consumer<IMessage<T>>
    {
        private static class MyHashMap<K,V> extends HashMap<K,V>{
            @Override
            public V get(Object key){
                V temp = super.get(key);
                try {
                    if (temp == null) temp = (V)Collections.emptyList();
                } catch (ClassCastException e) {}
                return temp;
            }
        }
        private Map<String, List<T>> mailBox;
        public MailService(){
            mailBox = new MyHashMap<>();
        }
        @Override
        public void accept(IMessage<T> t){
            if(mailBox.containsKey(t.getTo())) {
                List<T> val;
                val = mailBox.get(t.getTo());
                val.add(t.getContent());
                mailBox.put(t.getTo(), val);
            } else {
                List<T> val;
                val = new LinkedList<>();
                val.add(t.getContent());
                mailBox.put(t.getTo(), val);
           }
        }
        public Map<String, List<T>> getMailBox() {
            return mailBox;
        }
    }

В конечном итоге, вам нужно реализовать классы MailService, MailMessage и Salary (и, вероятно, вспомогательные классы и интерфейсы) и отправить их код в форму. Все классы должны быть публичными и статическими (ваши классы подставятся во внешний класс для тестирования).
В идеологически правильном решении не должно фигурировать ни одного оператора instanceof.
В классе для тестирования объявлены следующие импорты:

    import java.util.*;
    import java.util.function.*;