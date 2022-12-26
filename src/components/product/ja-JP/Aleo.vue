<template>
  <ProductPage
    :good-id='goodID'
    :purchase-amount='purchaseAmount'
    project-class='project-aleo'
    bg-img='product/aleo/aleo-banner.jpg'
    :customize-info='true'
  >
    <template #product-info>
      <div class='three-section'>
        <h4>{{ $t('MSG_PRICE') }}:</h4>
        <span class='number'>{{ good.getPrice(goodID) }}</span>
        <span class='unit'>{{ PriceCoinName }}</span>
        <div class='tooltip'>
          <img class='more-info' :src='question'><span>{{ $t('MSG_LEARN_MORE') }}</span>
          <p class='tooltip-text'>
            {{ $t('MSG_PRICE_TIP') }}
          </p>
        </div>
      </div>
      <div class='three-section'>
        <h4>{{ $t('MSG_DAILY_MINING_REWARDS') }}:</h4>
        <span class='number'>*</span>
        <span class='unit'>{{ target?.CoinUnit }} / {{ $t('MSG_DAY') }}</span>
        <div class='tooltip'>
          <img class='more-info' :src='question'><span>{{ $t('MSG_LEARN_MORE') }}</span>
          <p class='tooltip-text'>
            {{ $t('MSG_DAILY_REWARD_TIP') }}
          </p>
        </div>
      </div>
      <div class='three-section'>
        <h4>{{ $t('MSG_SERVICE_PERIOD') }}:</h4>
        <span class='number'>{{ target?.DurationDays }}</span>
        <span class='unit'>{{ $t('MSG_DAYS') }}</span>
        <div class='tooltip'>
          <img class='more-info' :src='question'><span>{{ $t('MSG_LEARN_MORE') }}</span>
          <p class='tooltip-text'>
            {{ $t('MSG_SERVICE_PERIOD_TIP') }}
          </p>
        </div>
      </div>
      <div class='three-section'>
        <h4>{{ $t('MSG_TECHNIQUE_SERVICE_FEE') }}:</h4>
        <span class='number'>20</span>
        <span class='unit'>%</span>
        <div class='tooltip'>
          <img class='more-info' :src='question'><span>{{ $t('MSG_LEARN_MORE') }}</span>
          <p class='tooltip-text'>
            {{ $t('MSG_TECHNIQUE_SERVICE_FEE_TIP') }}
          </p>
        </div>
      </div>
      <div class='three-section'>
        <h4>{{ $t('MSG_ORDER_EFFECTIVE') }}:</h4>
        <span class='number'>{{ true ? 'TBD*' : good.getFormatTime(target?.StartAt) }}</span>
        <div class='tooltip'>
          <img class='more-info' :src='question'><span>{{ $t('MSG_LEARN_MORE') }}</span>
          <p class='tooltip-text'>
            {{ $t('MSG_ORDER_EFFECTIVE_TIP') }}
          </p>
        </div>
      </div>
      <div class='three-section'>
        <h4>{{ $t("MSG_SALE_END_DATE") }}</h4>
        <span class='number'>2023-01-27</span>
        <span class='unit'>08:59 {{ $t("MSG_JST") }}</span>
        <div class='tooltip'>
          <img class='more-info' src='font-awesome/question.svg'><span>{{ $t('MSG_LEARN_MORE') }}</span>
          <p class='tooltip-text'>
            {{ $t('MSG_SALE_END_DATE_TIP') }}
          </p>
        </div>
      </div>
    </template>
    <template #product-detail>
      <div v-show='coinDescription'>
        <h3>{{ coinDescription ? $t(coinDescription?.Title) : '' }}</h3>
        <p v-html='coinDescription ? $t(coinDescription?.Message) : ""' />
      </div>

      <h3>なぜAleoなのか？</h3>
      <p>Aleoは、完全なプライベートアプリケーションを提供する史上初のプラットフォームです。Aleoは、分散型システムとゼロ知識暗号技術を活用し、ウェブ上のユーザーデータを保護することで、プライバシーを実現します。</p>
      <p>ゼロ知識暗号技術を使用することで、スマートコントラクトの実行をオフチェーンに移行し、完全なプライベート性と毎秒数万件のトランザクションに対応できる多様な分散型アプリケーションを可能にします。オープンなパブリックブロックチェーン上に構築されたAleoは、Ethereumの柔軟性をすべて備えていますが、マイナーがすべての取引を再実行する必要がなく、その正しさを検証するだけという、よりスケーラブルな構造を備えています。</p>
      <h4>FILECOINより優れた、WEB3の新世代</h4>
      <p>AleoはシリーズB資金調達ラウンドで約255億円 (2億ドル) を調達し、上場前の評価額は約1852億円 (14億5000万ドル) に達しました。この資金調達は、ゼロ知識証明技術の業界において過去最大の資金調達ラウンドとなります。</p>
      <p>現在(2022年4月時点)、総資金調達額は約289億円 (2億2800万ドル) を超え、Filecoinの約327億円 (2億5700万ドル) に迫る勢いです。</p>

      <h3>ベンチャーキャピタルからの投資</h3>
      <p>この資金調達は、ゼロ知識証明技術の業界において過去最大の資金調達ラウンドとなります。 <a href='https://www.crunchbase.com/search/principal.investors/field/organizations/num_investors/aleo-673f' target='_blank'>出資者には、ソフトバンク、Coinbase、a16z、その他19社以上の世界的な投資家が含まれています。</a></p>
      <p>投資家は、Aleoが完全にプライベートなWeb3の実現という使命を果たすことに大きな期待を寄せています。Ethereum上に構築されているプロジェクトの中には、Ethereumのスケーラビリティの問題を解決するためにゼロ知識証明（ZKP）を使用しているものがあります。一方、Aleoは独自のレイヤー1のブロックチェーンを開発し、スケーラビリティとプライバシーの両方を向上させます。a16zによると、「AleoはZexeと呼ばれる新しい計算システムを使用しているがゆえ完全にプライベートで且つ、現在稼働している多くのブロックチェーンをはるかに超えるスケーラビリティを実現する」とのことです。</p>
      <div class='logo-display'>
        <div><img src='vc-logos/softbank-vision-fund.png'></div>
        <div><img src='vc-logos/a16z.png'></div>
        <div><img src='vc-logos/polychain-capital.png'></div>
        <div><img src='vc-logos/coinbase.png'></div>
        <div><img src='vc-logos/samsung-next.png'></div>
        <div><img src='vc-logos/slow-ventures.png'></div>
        <div><img src='vc-logos/dekrypt-capital.png'></div>
        <div><img src='vc-logos/kora.png'></div>
        <div><img src='vc-logos/sea-capital.png'></div>
        <div><img src='vc-logos/tiger-global.png'></div>
        <div><img src='vc-logos/zkv.png'></div>
        <div><img src='vc-logos/placeholder.png'></div>
        <div><img src='vc-logos/scalar-capital.png'></div>
        <div><img src='vc-logos/galaxy-digital.png'></div>
        <div><img src='vc-logos/variant.png'></div>
        <div><img src='vc-logos/ethereal-ventures.png'></div>
        <div><img src='vc-logos/a_capital.png'></div>
        <div><img src='vc-logos/david-namdar.png'></div>
        <div><img src='vc-logos/balaji-srinivasan.png'></div>
      </div>

      <h3>Aleoのゼロ知識証明技術は、なぜWeb3にとって重要なのか？</h3>
      <p>Web2においてユーザーは、プライバシー情報を企業に委ねてしまっていました。データの漏洩、個人データの売買、漏洩したデータを永続的に利用されてしまうことなどは、すべてWeb2の世界の大きな欠点と言えるでしょう。</p>
      <p>Web3は、所有権やプライバシーを放棄したり、仲介者に頼ることなく、ユーザー同士がピアツーピアでやりとりできる、オープンでトラストレス（仲介者不要取引）かつパーミッションレス（管理者の許可なく誰でもアクセス可能）なインターネットを実現します。Aleoは、ゼロ知識証明（ZKP)により、Web3の未来の最も重要なインフラの1つを提供します。ZKPを使用してプライバシーとプログラマビリティの両方を達成し、第三者が情報を直接明かす必要なく、情報の真偽を確認することを可能にします。</p>
      <p>例えば、NFTコレクションを作成し、各自が1つのNFTしか所有できないと仮定します。この場合、Web2では各自がNFTを保有するためにユーザーが自分の個人情報とNFTを公開する必要があります。しかし、Web3上にてZKPsを活用することで、ユーザーは自分が誰であるかを具体的に明かすことなく、自分の身元を証明することができ、完全にプライバシーを保護しつつ、同じ保証を得ることができます。簡単な取引の例としては、自分の名前や購入履歴を明かさず、匿名でクレジットカードで支払うことを想像してみてください。</p>
      <p>Aleoの長期的な目標としては、銀行口座やオンラインショッピング、本人確認、SNS、ビジネスなど、日常で利用するケースにプライバシー機能を導入し、誰もが自分自身のデータを所有し保護できる、よりプライベートで安全な世界を実現することを目指しています。</p>
      <h3>Aleo チームメンバー</h3>
      <p>Aleoは、優秀なチームによって開発されています。2019年にHoward Wu、Michael Beller、Collin Chin、Raymond Chuによって法人を創設。Aleoのチームには、Google、Amazon、Facebookなどの企業や、UC バークレー、ジョンズ・ホプキンズ大学、ニューヨーク大学、コーネル大学などの研究に長けた大学から、世界トップの暗号技術者およびエンジニア、デザイナー、オペレーターが集結しています。</p>
      <div class='project-team'>
        <div class='team-member'>
          <h4 class='name'>
            Howard Wu
          </h4>
          <h5 class='title'>
            共同創業者
          </h5>
          <p class='bio'>
            UCバークレー校を卒業後、Googleにエンジニアとして入社。その後バークレー校のブロックチェーンアクセラレーターとして務め、Dekrypt Capitalでは10年以上ブロックチェーンに携わる。
          </p>
        </div>

        <div class='team-member'>
          <h4 class='name'>
            Collin Chin
          </h4>
          <h5 class='title'>
            共同創業者
          </h5>
          <p class='bio'>
            UCバークレー校在学中にConsenSysでスマートコントラクト開発者としてインターンを経験。Blockchain at BerkeleyとArgusでは、テクニカルアドバイザー兼セキュリティコンサルタントを務める。
          </p>
        </div>

        <div class='team-member'>
          <h4 class='name'>
            Raymond Chu
          </h4>
          <h5 class='title'>
            共同創業者
          </h5>
          <p class='bio'>
            UCバークレー校在学中、IBMとAmazonでエンジニアとしてインターンを経験。その後、Blockchain at BerkeleyとArgusでプロジェクト・マネージャー兼セキュリティ・コンサルタントを務める。
          </p>
        </div>

        <div class='team-member'>
          <h4 class='name'>
            Michael Beller
          </h4>
          <h5 class='title'>
            CFO
          </h5>
          <p class='bio'>
            アクセンチュアで7年間シニアマネージャーを務める。数々のIT/金融系スタートアップでCOO/CIOを歴任。
          </p>
        </div>
      </div>
      <h3>Aleoトークンエコノミー</h3>
      <p><img class='project-promo-content' :src='locale === "ja-JP" ? "product/aleo/aleo-posw-jp.jpg" : "product/aleo/aleo-posw-en.jpg"'></p>
      <p>
        <a href='https://coingenius.news/tokenomics-aleo/'>{{ $t('MSG_ALEO_TOKENOMICS_COINGENIUS_NEWS') }}</a>
      </p>
      <p>Aleoのトークンエコノミーは、全てのステークホルダーのインセンティブをシンプルに調整し、Aleoのネットワークに貢献と利益をもたらす設計となっています。これを実現するために、Aleoは下記目標を掲げています。</p>
      <ul>
        <li>トークンが価値を維持できるようにする</li>
        <li>ネットワークのセキュリティに貢献するすべての参加者にインセンティブを与える</li>
        <li>インセンティブによる不正行為につながることを防止する</li>
        <li>有用なアプリケーションのエコシステムを構築する</li>
        <li>分散化を促進する</li>
      </ul>
      <p>そのため、Aleoは 1 ブロックあたり 100 Aleo トークンのマイニング報酬で開始する方針で、最初の3年間は年間約 1億6000万 Aleo トークンがマイニングされると試算しています。上図のように、4年目には報酬が半分になり、そして7年目にも再び半分になります。Aleoブロックチェーンが誕生して10年目には最後の半減期で再び半分になります。</p>
      <p>ブロックチェーンの開始時に生成される10億 Aleo トークンの初期供給量を合わせると、20億 Aleo トークンあたりで供給量が横ばいになることを意味します。これは、Aleoの供給量に上限がないことを意味するが、Aleo チームによれば、最初の10年以降のトークン発行枚数は、健全なトークンインフレであるとのことです。</p>
      <p>ファーストバッチのマイナーにとっては、最初の3年間のAleoマイニングが最も報酬を生むことになります。この事実と、新しいトークンは高い価値で始まり、徐々に低下する傾向があるという一般的な傾向を組み合わせると、ファーストバッチマイニングが最も利益率が高いことが証明されるはずです。</p>

      <h3>Aleoと 競合プロジェクトの比較</h3>
      <p>Aleoは、レイヤー1 の技術により、次のEthereumやSolanaになりえる可能性を秘めています。今後10年間でプライバシー保護が更に重視されるため、長期的な成長が期待されます。Aleoは、あらゆるアプリケーションのプライバシー保護に利用されることを想定しており、様々なケースでの実装が可能となります。</p>
      <p><img class='project-promo-content' :src='locale === "ja-JP" ? "product/aleo/aleo-comparison-jp.png" : "product/aleo/aleo-comparison-en.png"'></p>
      <h4>プライバシー保護のためのゼロ知識証明</h4>
      <p>ゼロ知識証明とは、ある個人情報についての記述が正しいことを、証明者の身分を明かすことなく、またその情報そのものも明かすことなく、他の者（検証者）に証明することができる暗号技術です。これによっていくつかの利点があります。</p>

      <ul>
        <li>取引のオフライン計算に関するすべての情報を隠します</li>
        <li>取引は、オフライン計算に関係なく、誰でもリアルタイムに検証することができます</li>
        <li>Aleoの取引は、オフライン計算に関係なく968bytesであり、その生成には1分もかかりません</li>
        <li>取引は「ガス」を使用しないため、アプリケーションの実行時間が無制限です</li>
        <li>ユーザー定義のファンジブルアセットや規制に適したプライベートなステーブルコインの分散型取引所が可能になります</li>
      </ul>

      <p>これらはすべて、Aleoのコンセンサス・アルゴリズムPoSW（Proof of Succinct Work）に集約されます。</p>
      <p><img class='project-promo-content' :src='locale === "ja-JP" ? "product/aleo/aleo-algorithm-jp.png" : "product/aleo/aleo-algorithm-en.png"'></p>
      <h3>Aleoの今後と将来性</h3>
      <p>現在(2022年6月時点)、Aleoは2021年後半から始まった第2テストネットの段階にあり、2022年には第3テストネットが予定されています。また、メインネットのローンチは、現在2022年第3四半期に予定されています。LPSecretBaseは12ヶ月以上にわたってAleoを調査しており、私たちのノードは2022年1月からAleoのテストネットで稼働しています。</p>
      <p>世界のトップクラスの投資家からの投資を受け、Aleoは世界有数の取引所にとってFilecoinと同様に魅力的であることが証明されています。メインネットのローンチ後、AleoはCoinbase、Gemini、Kraken、Binance、Huobi、OKEx、その他多くの取引所に上場することが予想されます。</p>

      <div class='logo-display'>
        <div><img src='vc-logos/coinbase.png'></div>
        <div><img src='vc-logos/gemini.png'></div>
        <div><img src='vc-logos/kraken.png'></div>
        <div><img src='vc-logos/binance.png'></div>
        <div><img src='vc-logos/huobi.png'></div>
        <div><img src='vc-logos/okex.png'></div>
      </div>
      <p>Aleoの競争力のある特性と、開発者にとって魅力的な機能を持つレイヤー1であることから、Aleoの高い普及が期待されます。</p>
      <p>マイニング量とROIに関しては、通常、新しいマイニングプロジェクトは初年度に最も高い価値を見出します。Aleoのメインネットの初年度の高いマイニング報酬と相まって、マイニングに有利な条件が揃っているはずです。</p>
      <p>LPSecretBaseのマイニングマシンは、すでにテストネットで稼働しています。Aleoは、テストネットからのハッシュパワーがメインネットに引き継がれることを明言しており、これはファーストバッチマイニングにとって重要なアドバンテージとなります。LPSecretBaseのマイニングマシンを管理している同じチームは、Chiaのマイニングでも実績があり、これらマイニングのスペシャリストは世界トップクラスです。</p>

      <h3>警告と免責事項：暗号資産はリスクを伴います</h3>
      <p>LPSecretBaseとそのチームは、第一にユーザーのために最善の努力をします。</p>
      <p>とはいえ、一般的な規定として、ユーザーは自身で知識のある、それに伴うリスクも理解している商品のみを選択する必要がございます。これらの商品は、政府の政策や経済状況、市場動向の変化など様々な要因により、価値が変動する可能性があります。また、当該商品は、当該商品が保有される海外の関連法域の適用法令の適用を受けます。最後に、お客様がオンラインプラットフォーム上で取引を行う場合、当社のコントロール外の状況により、ハードウェアやソフトウェアの予測不可能な故障を含むシステムに関連するリスクにさらされる可能性があります。</p>
    </template>
    <template #sidebar>
      <div id='project-links' class='product-sidebar-section'>
        <h3 class='form-title'>
          {{ $t('MSG_PROJECT_LINKS') }}
        </h3>
        <ul class='product-links'>
          <li>
            <a href='https://www.aleo.org/' target='_blank'><img class='link-icon' src='font-awesome/homepage.svg'>
              <span>{{ $t('MSG_ALEO_HOMEPAGE') }}</span>
            </a>
          </li>
          <li>
            <a href='https://twitter.com/AleoHQ' target='_blank'><img class='link-icon' src='font-awesome/twitter.svg'>
              <span>{{ $t('MSG_TWITTER') }}</span>
            </a>
          </li>
          <li>
            <a href='https://www.youtube.com/channel/UCS_HKT2heOC_q88YQLiJt0g' target='_blank'><img class='link-icon' src='font-awesome/youtube.svg'>
              <span>{{ $t('MSG_YOUTUBE') }}</span>
            </a>
          </li>
          <li>
            <a href='https://discord.com/invite/aleohq' target='_blank'><img class='link-icon' src='font-awesome/discord.svg'>
              <span>{{ $t('MSG_DISCORD') }}</span>
            </a>
          </li>
          <li>
            <a href='https://github.com/AleoHQ' target='_blank'><img class='link-icon' src='font-awesome/github.svg'>
              <span>{{ $t('MSG_GITHUB') }}</span>
            </a>
          </li>
        </ul>
      </div>
      <!-- <div id='help' class='product-sidebar-section'>
        <h3 class='form-title'>
          {{ $t('MSG_PRODUCT_FAQ') }}
        </h3>
        <ul class='product-links'>
          <li>
            <a href='https://www.youtube.com/watch?v=xHIXUVhCLMY' target='_blank'>
              <img class='link-icon' :src='lightbulb'>
              <span>{{ $t('MSG_PURCHASE_ALEO_METHOD') }}</span>
            </a>
          </li>
          <li>
            <a href='https://www.youtube.com/watch?v=CW2ndiAvYD0' target='_blank'>
              <img class='link-icon' :src='lightbulb'>
              <span>{{ $t('MSG_MOBILE_PURCHASE_ALEO_METHOD') }}</span>
            </a>
          </li>
          <li>
            <a href='#/product/aleo'>
              <img class='link-icon' :src='lightbulb'>
              <span>{{ $t('MSG_PDF_MANUAL') }}</span>
            </a>
          </li>
        </ul>
      </div> -->
    </template>
  </ProductPage>
</template>

<script setup lang='ts'>
import { defineAsyncComponent, computed, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import { PriceCoinName } from 'npool-cli-v2'
import { useI18n } from 'vue-i18n'

import question from '../../../assets/question.svg'
// import lightbulb from '../../../assets/lightbulb.svg'
import { DefaultGoodID } from 'src/const/const'
import { AppGood, NotifyType, useAdminAppGoodStore, useAdminCoinDescriptionStore, CoinDescriptionUsedFor, useAdminCurrencyStore } from 'npool-cli-v4'
import { getCurrencies, getDescriptions } from 'src/api/chain'

// eslint-disable-next-line @typescript-eslint/unbound-method
const { t } = useI18n({ useScope: 'global' })

const ProductPage = defineAsyncComponent(() => import('src/components/product/ProductPage.vue'))

interface Query {
  goodId: string;
  purchaseAmount: number;
}

const { locale } = useI18n({ useScope: 'global' })

const route = useRoute()
const query = computed(() => route.query as unknown as Query)
const goodID = computed(() => query.value.goodId?.length ? query.value.goodId : DefaultGoodID)
const purchaseAmount = computed(() => query.value.purchaseAmount)

const good = useAdminAppGoodStore()
const target = computed(() => good.getGoodByID(goodID.value) as AppGood)

const currency = useAdminCurrencyStore()

const description = useAdminCoinDescriptionStore()
const coinDescription = computed(() => description.getCoinDescriptionByCoinUsedFor(target.value?.CoinTypeID, CoinDescriptionUsedFor.ProductPage))

onMounted(() => {
  good.getAppGood({
    GoodID: goodID.value,
    Message: {
      Error: {
        Title: t('MSG_GET_GOOD'),
        Message: t('MSG_GET_GOOD_FAIL'),
        Popup: true,
        Type: NotifyType.Error
      }
    }
  }, () => {
    // TODO
  })

  if (description.CoinDescriptions.CoinDescriptions.length === 0) {
    getDescriptions(0, 100)
  }
  if (currency.Currencies.Currencies.length === 0 || currency.expired()) {
    currency.$reset()
    getCurrencies(0, 100)
  }
})

</script>

<style lang='sass' scoped>
</style>
