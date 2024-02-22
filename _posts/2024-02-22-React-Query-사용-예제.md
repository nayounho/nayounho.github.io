---
title: "[React-query] React-query 사용예제"
date: 2024-02-22 20:00:00 +09:00
categories: [Development, Library]
tags: [React-query, React]
render_with_liquid: false
---

# React-query 사용 예제

## 기존 코드

```

const Coins = () => {

const [coins, setCoins] = useState<ICoin[]>([]); // 통신으로 받아온 coins data 상태관리

const [loading, setLoading] = useState(true); // 통신 과정 상태관리



useEffect(() => { // 화면 mount 시 해당 함수 실행

(async () => {

const res = await fetch("httpspi.coinpaprika.com/v1/coins"); // data 요청

const data = await res.json();



setCoins(data.slice(0, 100)); // api 요청으로 받아온 data 상태 업데이트

setLoading(false); // 상태 업데이트 후 통신 과정 업데이트

})();

}, []);



<Container>

<Header>

  <Title>코인</Title>

</Header>

// loading의 boolean 값을 보고 화면 rander 결정
  {loading ? (

  <Loader>loading...</Loader>

  ) : (

    <CoinList>

    // 받아온 data를 뿌려주는 과정
      {data?.slice(0, 100).map((coin) => (

        <Coin key={coin.id}>

          <Link to={`/${coin.id}`} state={{ name: coin.name }}>

              <Img

              src={`https://cryptocurrencyliveprices.com/img/${coin.id}.png`} alt=""

              />

              {coin.name} &rarr;

            </Link>

          </Coin>

        ))
      }

    </CoinList>

  )
}

</Container>
  );
};

```

## React Query 코드

```

// src/api.ts

const BASE_URL = "https://api.coinpaprika.com/v1";



export const fetchCoins = () => {

  return fetch(`${BASE_URL}/coins`).then((res) => res.json());

};

```

```

// src/routes/Conins.tsx


const Coins = () => {

const { isLoading, data } = useQuery<ICoin[]>('allCoins', fetchCoins);

// isLoading: boolean값으로 반환

// data: api 통신으로 받아온 data를 반환

// allCoins: 고유한 값 지정

// fetchCoins: api라는 파일을 따로 만든 후 그곳에서 해당 api 통신을 진행 한 함수를 등록

<Container>

  <Header>

    <Title>코인</Title>

  </Header>

  {isLoading ? ( // isLoading의 boolean 값을 보고 화면 rander 결정

    <Loader>loading...</Loader>

    ) : (

    <CoinList>

      // 받아온 data를 뿌려주는 과정
      {data?.slice(0, 100).map((coin) => (

        <Coin key={coin.id}>

          <Link to={`/${coin.id}`} state={{ name: coin.name }}>

            <Img

            src={`https://cryptocurrencyliveprices.com/img/${coin.id}.png`} alt=""
            />

            {coin.name} &rarr;

          </Link>

        </Coin>
        ))
      }

    </CoinList>

    )}

</Container>
  );
};

```

## api 통신을 2번 해야하는 상황

```

// src/api.ts



const BASE_URL = "https://api.coinpaprika.com/v1";



export const fetchCoinInfo = (coinId: string) => {

return fetch(`${BASE_URL}/coins/${coinId}`).then((res) => res.json());

};



export const fetchCoinTickers = (coinId: string) => {

return fetch(`${BASE_URL}/tichers/${coinId}`).then((res) => res.json());

};



```

```

// src/routes/coin.tsx



const Coin = () => {

  const { isLoading: infoLoading, data: infoData } = useQuery<InfoData>(

      ["info", coinId],

      () => fetchCoinInfo(coinId)

    );

  const { isLoading: tickersLoading, data: tickersData } = useQuery<PriceData>(

      ["tickers", coinId],

      () => fetchCoinTickers(coinId)

    );



// isLoading, data의 이름이 중복되기 때문에 각각의 고유한 이름으로 설정

// ["info", coinId] -> 고유한 값 설정 부분에서도 coinId가 중복되기 때문에 고유한 값을 같이 설정

// 함수 등록 과정에서도 해당 함수에 coinId라는 인수가 필요하여 함수 자체를 설정



const loading = infoLoading || tickersLoading; // infoLoading, tickersLoading이

둘 다 true가 나올때만 실행

```

## 사용 후기

- 사용성: 사용성 측면에서 굉장히 편한 사용 방법과 코드의 양을 줄여줄 수 있었다. 상태관리가 따로 필요하지 않고

isLoading이라는 boolean값을 반환하기 때문에 통신에 따라 화면 설정 과정에서도 편리했다

- 성능: 성능 또한 받아온 data를 cashing하는 기능이 있기 때문에 재렌더링 시 통신이 일어나지 않아

성능 상으로도 뛰어나다고 생각한다.

- api 통신에서 React Query를 적극 활용할 생각이다.
