# Practice

6. 1 Generics

Предположим, у нас есть параметризованный класс

    public class Example<X> {
       ...
    }

Что можно подставлять в качестве значения параметра X при использовании этого класса в программе?

 - значение примитивного типа (например, 42) 
 - имя любого примитивного типа (например, int)
 - имя любого интерфейса (например, CharSequence) (+)
 - имя любого перечисления (например, DayOfWeek) (+)
 - ссылку на метод (например, Object::toString) 
 - имя любого класса (например, Object) (+)
 - значение X можно не указывать, т.е. использовать класс Example как обычный непараметризованный (+)
 - символ "?" или более сложное выражение с ключевыми словами extends и super (+)
 
Задача №1
 
Реализуйте generic-класс Pair, похожий на Optional, но содержащий пару элементов разных типов и не запрещающий элементам принимать значение null.
Реализуйте методы getFirst(), getSecond(), equals() и hashCode(), а также статический фабричный метод of(). Конструктор должен быть закрытым (private).
С корректно реализованным классом Pair должен компилироваться и успешно работать следующий код:

    Pair<Integer, String> pair = Pair.of(1, "hello");
    Integer i = pair.getFirst(); // 1
    String s = pair.getSecond(); // "hello"

    Pair<Integer, String> pair2 = Pair.of(1, "hello");
    boolean mustBeTrue = pair.equals(pair2); // true!
    boolean mustAlsoBeTrue = pair.hashCode() == pair2.hashCode(); // true!

Пожалуйста, не меняйте модификатор доступа класса Pair. Для корректной проверки класс должен иметь пакетную видимость.

    class Pair <X, Y>{
        private Pair(X x, Y y){
            firstValue = x;
            secondValue = y;
        }
        private X firstValue;
        private Y secondValue;

        public static <X, Y> Pair<X, Y> of(X x, Y y) {
           return new Pair(x, y);
       }
       public X getFirst() {
           return firstValue;
       }
        public Y getSecond() {
           return secondValue;
           }
        public boolean equals(Object other){
            if (this == other) {
                return true;
            }
            if (Pair.class.isInstance(other)) {
                return Objects.equals(firstValue, ((Pair<?,?>)other).firstValue) &&
                    Objects.equals(secondValue, ((Pair<?,?>)other).secondValue);
            }
            return false;
       }
       public int hashCode() {
            return Objects.hashCode(this.getFirst()) ^ Objects.hashCode(this.getSecond());
       }
    }
    
Если бы в классе Pair мы захотели написать метод ifPresent, аналогичный одноименному методу класса Optional и принимающий java.util.function.BiConsumer, то как надо было бы объявить такой метод?

    class Pair<T, U> {
        // вариант 1
        public void ifPresent(BiConsumer<T, U> consumer) { ... }

        // вариант 2
        public void ifPresent(BiConsumer<? super T, ? super U> consumer) { ... }

        // вариант 3
        public void ifPresent(BiConsumer<? extends T, ? extends U> consumer) { ... }

        // вариант 4
        public void ifPresent(BiConsumer<?, ?> consumer) { ... }

        // вариант 5
        public <X, Y> void ifPresent(BiConsumer<X, Y> consumer) { ... }
}

 - вариант 1
 - вариант 2 (+)
 - вариант 3
 - вариант 4
 - вариант 5
 
6. 2 Коллекции

Чем коллекции отличаются от массивов?
 - Массивы встроены в язык, а коллекции — обычные классы стандартной библиотеки. (+)
 - Коллекции могут содержать только объекты, а массивы могут содержать и примитивы. (+)
 - Массивы можно использовать в цикле foreach, а коллекции нельзя.
 - Коллекции можно сравнивать по содержимому обычным методом equals(), а для сравнения массивов используется внешний утилитный метод Arrays.equals(). (+)
 - Коллекции могут динамически менять размер, а размер массива фиксируется при создании. (+)
 
Предположим, у нас есть две переменных:

    Collection<?> collection = ...;
    Object object = ...;

Какие операции над collection допустимы?

 - collection.add(object)
 - collection.toArray() (+)
 - collection.addAll(Arrays.asList(object))
 - collection.size() (+)
 - collection.remove(object) (+)
 - collection.iterator() (+)
 - collection.contains(object) (+)
 - collection.clear() (+)
 
В следующей программе

    Set<String> set = new LinkedHashSet<>();
    // add some elements to the set
    Iterator<String> iterator = set.iterator();

в каком порядке iterator будет возвращать элементы множества?

 - В обратном лексикографическом порядке
 - В непредсказуемом порядке, основанном на хеш-кодах строк
 - В лексикографическом порядке
 - В порядке добавления (+)
 
Как должны быть связаны между собой реализации equals() и hashCode() у класса, чтобы экземпляры этого класса можно было спокойно хранить в HashSet?

 - если a.equals(b), то a.hashCode() == b.hashCode() (+)
 - a.equals(b) тогда и только тогда, когда a.hashCode() == b.hashCode()
 - если !a.equals(b), то a.hashCode() != b.hashCode()
 - если a.hashCode() == b.hashCode(), то a.equals(b)
 
Задача №1
Реализуйте метод, вычисляющий симметрическую разность двух множеств.
Метод должен возвращать результат в виде нового множества. Изменять переданные в него множества не допускается.

    public static <T> Set<T> symmetricDifference(Set<? extends T> set1, Set<? extends T> set2) {
        Set<T> result = new HashSet<>();
        for(T s1 : set1) {
            if (!set2.contains(s1)){
                result.add(s1);
            }
        }
        for(T s2 : set2) {
            if (!set1.contains(s2)){
                result.add(s2);
            }
        }
        return result;
    }
    
Задача №2

Напишите программу, которая прочитает из System.in последовательность целых чисел, разделенных пробелами, затем удалит из них все числа, стоящие на четных позициях, и затем выведет получившуюся последовательность в обратном порядке в System.out.
Все числа влезают в int. Позиции чисел в последовательности нумеруются с нуля.
В этом задании надо написать программу целиком, включая import'ы, декларацию класса Main и метода main.

    import java.io.*;
    import java.text.DecimalFormat;
    import java.util.*;

    public class Main {

        public static void main(String[] args) throws IOException {
            double sum = 0;
            String str = readString(System.in);

            ArrayList<String> collection = new ArrayList<>();

            List list = Arrays.asList(str.split("\\s"));
            Iterator<String> it = list.iterator();
            int idx = 0;
            while (it.hasNext()) {
                String element = it.next();
                if (idx++ % 2 == 0) continue;
                collection.add(element);
            }
            ListIterator lit = collection.listIterator(collection.size());
            while(lit.hasPrevious()) {
                String element = (String)lit.previous();
                System.out.print(element + " ");
            }
        }
        static String readString(InputStream is) throws IOException {
            char[] buf = new char[2048];
            Reader r = new InputStreamReader(is, "UTF-8");
            StringBuilder s = new StringBuilder();
            while (true) {
                int n = r.read(buf);
                if (n < 0)
                    break;
                s.append(buf, 0, n);
            }
            return s.toString();
        }
    }
