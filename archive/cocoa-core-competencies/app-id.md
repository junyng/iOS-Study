# App ID

앱 ID는 단일 개발팀에서 하나 이상의 앱을 식별하는 데 사용되는 두 부분의 문자열이다. 문자열은 팀 ID와 번들 ID 검색 문자열로 구성되며, 두 부분을 구분하는 마침표\(.\)가 있다. 팀 ID는 애플에서 공급하며 특정 개발 팀에만 고유하며, 번들 ID 검색 문자열은 단일 앱의 번들 ID 또는 애플리케이션 그룹의 번들 ID 집합에 일치하도록 사용자가 공급한다.

![](../../.gitbook/assets/app_ids_2x.png)

앱 ID에는 명시적 앱 ID, 단일 앱에 사용되는 와일드카드 앱 ID의 두 종류가 있다.

### An Explicit App ID Matches a Single App

앱과 일치하는 명시적 앱 ID를 얻으려면 앱 ID의 팀 ID가 앱과 연결된 팀 ID와 같아야 한다. 그리고 번들 ID 검색 문자열은 앱의 번들 ID와 같아야 한다. 번들 ID는 단일 앱을 식별하는 고유 식별자로 다른 팀에서는 사용할 수 없다.

### Wildcard App IDs Match Multiple Apps

와일드카드 앱 ID는 번들 ID 검색 문자열의 마지막 부분으로 별표를 포함하고 있다. 별표는 검색 문자열에 있는 번들 ID의 또는 전부를 대체한다.

![](../../.gitbook/assets/app_ids_wildcard_2x.png)

번들 ID 검색 문자열과 번들 ID를 일치시킬 때 별표는 와일드카드로 처리된다. 와일드카드 앱 ID가 앱 집합과 일치하려면 번들 ID 검색 문자열에서 별표 앞의 모든 문자와 정확히 일치해야 한다. 별표는 번들 ID에 남아 있는 모든 문자와 일치한다. 별표는 번들 ID의 하나 이상의 문자와 일치해야 한다. 아래 표에는 번들 ID 검색 문자열과 일부 일치 및 불일치 번들 ID가 표시된다.

| com.domain.\* |  | \(bundle id search string\) |
| :--- | :--- | :--- |
| com.domain.text | O | \* matches text. |
| com.domain.icon | O | \* matches icon |
| com.otherdomain.database | X | The d in the pattern fails to find a match. |
| com.domain | X | The . in the pattern fails to find a match. |
| com.domain. | X | The \* in the pattern fails to match a character. |

와일드카드 앱 ID가 앱과 일치하려면 팀 ID가 정확히 일치해야하며, 번들 ID가 와일드카드 일치 규칙을 사용하여 번들 ID 검색 문자열과 일치해야 한다.

#### Related Articles

\(None\)

#### Definitive Discussion

App Distribute Guide

