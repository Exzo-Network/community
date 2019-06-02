# 청산\(Liquidation\)

## 청산이란 무엇인가요?

CDP가 안전하지 않은것으로 간주된다면 청산될 수 있습니다. 이는 Dai의 가치를 보증하기 위하여, 시스템에 충분한 담보가 있다는 것을 보장하기 위한 것입니다. 청산은 CDP가 축적하고 있는 담보의 전체 가치\(오라클에 의해 판단됨\)가, 자산 규모에 따라 요구되는 담보 가치에 미치지 못하면 발생합니다. 청산 시, 부채와 수수료를 감당할 수 있는 양의 담보가 묶이고, 나머지는 유저가 출금할 수 있는 상태로 남게됩니다.

## 청산은 왜 존재하나요?

정부가 가치를 보증하는 법정화폐들과는 달리, Dai는 현대적이고 암호학적으로 보안된 _대표 화폐_의 화신입니다; 시장에 순환되고 있는 Dai는 모두 스마트컨트랙트에 묶여있는 초과 담보가 그 가치를 보증하며, 상대방을 신뢰할 필요가 없고, 계약 상대방에 의해 발생할 수 있는 리스크가 없습니다. 즉, 유저가 신뢰할 수 있는 완전히 투명한 시스템을 가지고 있습니다.

과잉 담보가 항상 존재한다는 것을 확실히 하기 위해, 키퍼\(Keeper\)라고 불리는 유저들이 CDP가 불안정해지거나, 담보가 충분치 않는지를 계속 확인합니다. 이 키퍼들은 Dai 스테이블코인 시스템 유저들 중에서 특별한 카테고리에 속합니다. 이들은 공급된 Dai를 모두 보증할만한 담보가 있어서 지불능력이 있는지를 확인함으로써 인센티브를 받습니다. 키퍼들은 위험한 CDP에 대해서는 빠르게 청산을 진행하여 전체 시스템이 건강하게 유지되도록 돕습니다. 이러한 행동은 시장이 빠르게 하락하면서 담보의 가치도 하락하여 부채를 상환하지 못하게 될 상황에 도달할때 특히 더 중요합니다.

## 청산 비율은 무엇인가요?

각 담보 수단은, 해당 자산의 리스크를 기반으로 측정된 특정한 청산 비율을 가지고 있습니다. 현재는 ETH만 담보로 취급되고 있고, 단일 담보 기반 Dai 시스템을 런칭할 때, Maker의 임시 리스크 팀은 ETH 자산에 대한 리스크 프로파일을 분석하여 청산 비율을 150%로 설정하였습니다.

탈중앙화된 오라클 피드\(Oracle Feed\)는 시스템에 ETH의 가격 데이터를 제공하여, 특정 CDP의 청산 비율이 시스템에서 요구되는 최저치에 도달하는 지를 확인하는 것을 돕습니다. 청산 비율은 리스크 프로파일과 오라클 피드에 의해 결정되는 시장 가격에 따라 결정됩니다.

예를 들어, 어떤 유저가 200 DAI를 대출하는데, 담보의 가치가 현재 시장가격의 50%이하로 떨어지지는 않을 것이라고 생각한다고 합시다. 그렇다면 유저는 적어도 최소 담보 비율의 두 배를 입금해야하는 것입니다. 현재 최소 담보 비율이 150%이므로, 유저는 $600 어치의 ETH를 입금하고 200 DAI를 대출받으면서 CDP 담보 비율을 300%로 유지하면 됩니다.

**이더리움 기반 CDP는 150%의 담보 비율이 키퍼들에 의해 청산당하지 않는 가장 낮은 수치라는 것을 알고있는 것이 매우 중요합니다.** 최소 기준 이상을 유지하는 것이 유저의 담보가 청산되는 것을 막을 수 있는 방법이기 때문입니다.

## 청산 가격은 무엇인가요?

잠금되어있는 담보가 청산되기 직전의 최소가격을 뜻합니다.

## 청산 페널티는 무엇인가요?

청산 페널티는 청산이 발생할 때, 전체 DAI 부채 상환과 더불어 추가되는 비용으로, 담보에서 자동으로 징수합니다.

페널티로 징수된 비용은 자동으로 PETH 풀로 전송됩니다. 이는 유저가 CDP에서 담보를 되돌려 받을 때 유저가 받는 WETH 비율을 증가시킵니다. 즉, 시장이 불안정하여 많은 청산이 발생했을 때 담보 풀의 가치를 올리게 됩니다.

## 청산 과정은 어떻게 이루어지나요?

청산은, 키퍼가 CDP를 폐쇄하고 유동성 제공 컨트랙트 \(Liquidity Providing Contract, LPC\)로 보낼 때 발생하며, Dai Dashboard에서 해당 CDP의 담보 자산을 팔게 됩니다. 담보 자산의 판매로 부채가 상환되면, 팔리지 않은 PETH 담보는 CDP 소유자에게 회수됩니다.

전체 과정은 다음과 같이 진행됩니다:

* 청산된 CDP가 종료됩니다
* 청산 페널티가 DAI 부채와 더불어 추가적용됩니다
* LPC는 현재의 오라클 가격을 기준으로, 부채를 상환할 수 있는 충분한 양의 PETH 담보를 제거합니다.
* CDP 소유자는 포지션이 종료되어 LPC에 의해 제거되고 남은 담보를 이동할 수 있습니다.
* 잠금되어있는 PETH는 [dai.makerdao.com](http://dai.makerdao.com)에서 [Boom/Bust Spread](http://glossary)라고 불리는 방식으로 가치를 조정하여, 할인된 가격으로 판매됩니다.
* PETH를 판매함으로써 발생하는 DAI는 CDP 부채를 상환하고, 소각됩니다.
* 만약 판매수익으로 발생한 DAI가 CDP 부채 상환 이후에도 남아있다면, 나머지는 PETH 재구매를 위해 판매되고 소각되며, 남겨진 PETH의 가치를 증가시킵니다.
* 만약 판매수익으로 발생한 DAI가 충분치 않다면, 새로운 PETH가 발행되어 부족분을 채우게 됩니다. 이것은 전체 풀의 가치를 희석시켜 시스템의 자본을 재구성합니다.

## 청산 이후에 담보는 얼마나 남아있나요?

다음의 단순한 공식을 통해서 청산 이후에 얼마만큼의 담보가 남아있을지를 계산해볼 수 있습니다:

`(담보 * 오라클 가격 * PETH/ETH 비율) - (청산 페널티 * 안정화 부채) – 안정화 부채 = (남은 담보 * 오라클 가격) DAI`

다음과 같은 상황이 주어질 경우:

* 1 ETH의 오라클 가격 = 350 USD
* 묶여있는 총 PETH = 10 ETH
* PETH/ETH 비율 = 1.012
* 청산 페널티 = 13%
* 1000 DAI의 안정화 부채

`(10 × 350 × 1.012) − (13% × 1000) − 1000 = 2412 DAI 혹은 6.891428571 ETH`

## 청산 가격은 어떻게 계산하나요?

다음의 단순한 공식을 통해서 담보의 가치가 얼만큼 하락해야 담보의 처분이 시작되는지를 계산해볼 수 있습니다:

`(안정화 부채 * 청산 비율) / (담보 * PETH/ETH 비율) = 청산 가격`

다음과 같은 상황이 주어질 경우:

* 1 ETH의 오라클 가격 = 350 USD
* 묶여있는 총 PETH = 12 ETH
* PETH/ETH 비율 = 1.012
* 청산 비율 = 150%
* 1000 DAI의 안정화 부채

  `(1000 × 1.5 ) ÷ (12 × 1.012) = 123.51 USD`

이더리움의 가격이 123.51 USD이하로 떨어지면 불안정한 상태로 간주되어 청산될 수 있습니다.

## 청산 이후에 담보는 얼마나 남아있나요?

다음의 단순한 공식을 통해서 청산 이후에 얼만큼의 담보가 남아있을지를 계산해볼 수 있습니다:

`(담보 * 오라클 가격 * PETH/ETH 비율) - (청산 페널티 * 안정화 부채) – 안정화 부채 = (남은 담보 * 오라클 가격) DAI`

다음과 같은 상황이 주어질 경우:

* 1 ETH의 오라클 가격 = 350 USD
* 묶여있는 총 PETH = 10 ETH
* PETH/ETH 비율 = 1.012
* 청산 페널티 = 13%
* 1000 DAI의 안정화 부채

`(10 × 350 × 1.012) − (13% × 1000) − 1000 = 2412 DAI 혹은 6.891428571 ETH`

## 청산 가격은 어떻게 계산하나요?

다음의 단순한 공식을 통해서 담보의 가치가 얼만큼 하락해야 담보의 처분이 시작되는지를 계산해볼 수 있습니다:

`(안정화 부채 * 청산 비율) / (담보 * PETH/ETH 비율) = 청산 가격`

다음과 같은 상황이 주어질 경우:

* 1 ETH의 오라클 가격 = 350 USD
* 묶여있는 총 PETH = 12 ETH
* PETH/ETH 비율 = 1.012
* 청산 비율 = 150%
* 1000 DAI의 안정화 부채

`(1000 × 1.5 ) ÷ (12 × 1.012) = 123.51 USD`

이더리움의 가격이 123.51 USD이하로 떨어지면 불안정한 상태로 간주되어 청산될 수 있습니다.

## 담보 비율은 어떻게 계산할 수 있나요?

만약 청산 가격이 아닌, 부채 대비 담보비율로 포지션이 안정한 정도를 확인하려면 다음의 간단한 수식을 사용하여 계산해볼 수 있습니다:

`(잠겨있는 PETH × ETH 가격 × PETH/ETH 비율) ÷ 안정화 부채 × 100 = 담보 비율`

다음과 같은 상황이 주어질 경우:

* 1 ETH의 오라클 가격 = 350 USD
* 묶여있는 총 PETH = 12 ETH
* PETH/ETH 비율 = 1.012
* 청산 비율 = 150%
* 1000 DAI의 안정화 부채

`(12 × 350 × 1.012) ÷ 1000 × 100 = 425.04%`

CDP는 425.04%의 담보비율을 가지고 있습니다.

## 청산 가격을 어떻게 하면 낮출 수 있나요?

예측하기 매우 어려운 시장에서 안전한 레버리지 포지션을 유지하는 것은, CDP 소유자들이 가장 먼저 겪게 되는 힘든 점입니다. 만약 CDP가 청산 가격에 가깝다면, 담보를 추가하거나, DAI를 상환하여 청산의 위험성을 낮춰야합니다.

어떤 방법을 선택할 것인지는 장기적 목표에 따라 달라질 수 있습니다. 만약 담보의 미래 가치가 떨어지지 않을것이라는 믿음이 있다면, 포지션에 담보를 추가하는 결정을 내릴 수 있습니다. 그렇지 않고 가격 변동 위험성을 줄이고 싶다면, CDP에 DAI를 상환하여 부채를 줄이는 방법을 사용할 수 있습니다.

청산의 위험성을 줄이는 가장 좋은 방법은 DAI를 상환하는 것으로, 이는 청산 가격을 훨씬 효율적으로 감소시키게 됩니다. 또한 이 방법은 해당 포지션에서 수수료를 줄이는 추가적인 효과도 있습니다.

다음과 같은 상황이 주어질 경우:

* 1 ETH의 오라클 가격 = 350 USD
* 묶여있는 총 PETH = 12 ETH
* PETH/ETH 비율 = 1.012
* 청산 비율 = 150%
* 1000 DAI의 안정화 부채
* 기존 청산 가격은 다음과 같습니다:

`(1000 × 1.5 ) ÷ (12 × 1.012) = 123.51 USD`

700 USD 가치를 담보에 추가할 경우:

`(1000 × 1.5 ) ÷ (14 × 1.012) = 105.87 USD`

700 USD 만큼의 부채를 상환했을 경우:

`(300 × 1.5 ) ÷ (12 × 1.012) = 37.05 USD`

위의 예시를 통해서 확인할 수 있듯이, DAI를 상환하는 것이 담보금을 추가하는 것보다 청산 가격을 낮추는데 훨씬 더 효율적입니다.

## 청산을 피하기 위한 일반적인 방법

CDP의 안정성을 유지하는 것은 해당 CDP 소유자의 책임임을 알고있으셔야 합니다. 자산이 청산으로부터 안전한 상태를 유지하는 것은 전적으로 CDP 유저에 의해 결정됩니다. 아래에 제시된 목록은 CDP의 안정성을 유지하기 위해 다른 유저들이 주로 사용하는 방법들입니다.

* 가장 좋아하는 앱이나, 여러개의 앱에 가격 알림을 설정하여 시장 상황에 대응할 수 있도록 합니다.
* CDP를 관리할 때 사용할 수 있는 추가 자산에 항상 접근할 수 있어야 합니다. 시장이 붕괴할때 아무것도 못하는 상황을 원하지 않을 것이기 때문입니다.
* 추가 자산을 준비하여 언제든 포지션에 담보를 추가하거나, 혹은 자산을 팔아서 DAI를 구매하여 부채를 줄일 수 있도록 해야합니다.
* 시장이 지속적으로 하락할 것으로 예측되면, CDP의 담보를 일부 회수하여 DAI를 사서 부채를 줄이는데 사용할 수 있습니다. 이 방법에서 조심해야 할것은 청산 가격에서 충분히 차이가 있는 상태에서 실행해야 한다는 것입니다. 왜냐하면, Dai로 상환하기 전까지 일시적으로 훨씬 더 위험한 포지션에 있게 되기 때문입니다.

CDP를 개설한다는 것은 리스크도 같이 발생하는것임을 명심해야합니다. 어느 정도의 리스크를 감당할 수 있을지는 여러가지 변수에 의해 결정됩니다. [리스크 프로파일\(risk profile\)](https://www.investopedia.com/terms/r/risk-profile.asp) 은 그 자체로도 복잡하지만, 모든 CDP 유저가 반드시 확인하고 넘어가야만 하는 것입니다.

리스크에 대한 추가 정보를 원하시면, 중요한 법적 정보를 담은 약관 [\(Terms of Service\)](https://cdp.makerdao.com/terms)를 참고해주세요. 모든 CDP 유저는 Dai 스테이블코인 시스템을 사용하기 전에 이 약관에 동의해야합니다.

## 스마트 컨트랙트가 어떻게 담보를 판매하나요?

키퍼가 불안정한 CDP를 청산하면, 유동성 제공 컨트랙트 \(Liquidity Providing Contract, LPC\)가 Dai 대시보드에 담보를 팔려고 내놓게 됩니다. 할인된 가격은 특별한 변경자가 적용된 오라클 피드 \(Oracle feeds\)에 의해 설정됩니다. 이 변경자는 상환해야 할 부채를 감안하여 가격을 할인합니다. 이 추가적인 ‘스프레드\(spread\)’는 담보를 구매자들에게 시장가격보다 저렴한 가격으로 제공함으로써, 자본의 재구성이 빠르게 이루어질 수 있도록 합니다.

유저들은 대시보드에서 LPC에 의해 잠금되어있는 PETH를 구매할 수 있습니다. 판매되고 남는 DAI는 PETH로 구매할 수 있습니다.

## 잠금되어있는 PETH를 살 수 있나요?

DAI 대시보드에 보시면, “강제 CDP 청산로부터 발생한 총 청산 가능량\(Total Liquidity Available from forced CDP liquidations\)” 섹션이 있습니다. 이곳에서 Bust/Boom 스프레드에 의해 정해진 할인 가격으로 잠금되어있는 PETH를 구매할 수 있습니다.

## "단기간의 심각한 폭락" 이 CDP의 청산에 어떻게 영향을 미치나요?

오라클은 여러 거래소의 가격을 종합하여 판단하기 때문에, 한 거래소에서 발생하는 단기간의 심각한 폭락은 시스템에 영향을 주지 않을 것입니다. 폭락이 감지되면, 중간값 계산자\(medianizer\)가 각 피드\(individual feed\) 의 중간값을 계산합니다. [https://mkr.tools/system/feeds](https://mkr.tools/system/feeds) 에서 모든 개별 오라클의 그래프를 확인하실 수 있습니다.

* 자세한 정보: [https://developer.makerdao.com/feeds/](https://developer.makerdao.com/feeds/)
* 피드의 코드: [https://github.com/makerdao/price-feed](https://github.com/makerdao/price-feed)
* 중간값 계산자 코드: [https://github.com/makerdao/medianizer](https://github.com/makerdao/medianizer)
* 업데이터 코드 \(Updater code\): [https://github.com/makerdao/setzer](https://github.com/makerdao/setzer)
* 피드의 확인: [https://mkr.tools/system/feeds](https://mkr.tools/system/feeds)

## 청산의 최신 정보들은 어디서 볼 수 있나요?

MakerDAO 시스템을 추적하는 제 3자가 제공하는 [mkr.tools](https://mkr.tools/)을 통해 확인하실 수 있습니다. 추가 정보를 얻을 수 있는 연관된 페이지는 [청산](https://mkr.tools/system/liquidations) 과 [바이트\(bites\)](https://mkr.tools/system/bites) 입니다.
