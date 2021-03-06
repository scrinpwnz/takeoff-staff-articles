## Презентационные компоненты и компоненты-контейнеры

Компоненты часто разделяют на два типа:

- Презентационные компоненты (глупые компоненты, чистые компоненты).
- Компоненты-контейнеры (умные компоненты, компоненты с состоянием).

---

### Презентационные компоненты

**Презентационные компоненты** в основном отвечают за внешний вид.
Они не управляют состоянием, за исключением состояния, которое нужно для этого вида.

Пример презентационного компонента:

```javascript
const Order = (props) => {
  const { number, recipient } = props;

  return (
    <li>
      <div>{number}</div>
      <div>{recipient}</div>
    </li>
  );
};
```

Он получает данные из `props` и сосредоточен на выводе элемента.

Аспекты присущие презентационному компоненту:

- сосредоточены на том как вещи выглядят.
- могут содержать в себе другие презентационные компоненты
- часто позволяют использовать `props.children`
- не зависят от остальной части приложения
- не определяют как данные загружаются и изменяются
- получают данные только через `props`
- редко имеют свое собственное состояние (если имеют, то только состояние влияющее на внешний вид)

---

### Контейнер-компоненты

**Компоненты-контейнеры** в основном отвечают за логику и управляют состоянием.
Они могут оборачивать несколько презентационных компонентов.

Пример компонента-контейнера:

```javascript
const OrdersContainer = () => {
  const [orders, setOrders] = useState(null);

  useEffect(() => {
    axios.get("api/orders").then((orders) => {
      setOrders();
    });
  });

  return (
    <ul>
      {orders.map((item) => (
        <Order number={item.number} recipient={item.recipient} />
      ))}
    </ul>
  );
};
```

Он управляет своим состоянием и передает данные в презентационные компоненты.

Аспекты присущие контейнер-компонентам:

- сосредоточены на том как вещи работают
- могут содержать в себе другие презентационные компоненты и контейнер-компоненты
- предоставляют данные и поведение для презентационных компонентов или других контейнер-компонентов
- могут вызывать `Flux` действия и предоставлять их как колбеки для презентационных компонентов
- часто имеют свое состояние
- обычно создаются через Компоненты Высшего Порядка (High Order Component)
