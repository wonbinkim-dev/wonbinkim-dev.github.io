# Day 01 · 중고거래 CLI 프로젝트 시작

## 프로젝트 목표
중고 거래 사이트를 둘러보던 중, 많은 사람들이 물건의 외관에
과도하게 집착한다는 느낌을 받았다.
개인적으로 중고거래는 작동만 잘 되면 된다고 생각하기 때문에,
외관보다는 작동 여부와 가격에 집중한 중고거래 사이트가
있다면 좋겠다고 생각했다.
그 아이디어를 직접 구현해보고자 이번 프로젝트를 시작했다.


현재 구현한 항목 

1. 물품 신규등록 
2. 등록된 물품의 정보 확인 
3. 등록된 품목 제거 
4. 프로그램 종료 이다.

## 기본 로직

아래는 현재까지 구현한 중고거래 CLI 프로그램의 기본 구조이다.
배열을 이용해 물품을 관리하며, 메뉴 선택에 따라 등록·조회·삭제 기능을 수행한다.

```c
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h> // rand, srand
#include <time.h>   // time
#include <string.h>

typedef struct {
    int id;
    char name[50];
    int price;
} Item;

int main(){
    srand(time(NULL));//같은 변수가 나오는걸 방지
    
    Item items[1000];
    int i = 0;

    while(true){
        printf("1. 신규등록 \n2. 품목보기 \n3. 품목 제거 \n4. 프로그램 종료\n\n");

        int menu = 0;
        printf("선택할 메뉴를 입력해 주세요: ");
        scanf("%d", &menu);
        getchar ();

        if(menu == 1){
            printf("신규등록 페이지, 계속 진행하려면 1을 뒤로가려면 5를 입력하세요.\n");

            int num = 0;
            scanf("%d", &num);
            getchar();

            if(num == 5) continue;

            items[i].id = rand() % 900000 + 100000;
            printf("물품의 id는 %d입니다\n", items[i].id);

            printf("물품의 이름을 입력하세요: ");
            fgets(items[i].name, sizeof(items[i].name), stdin);
            items[i].name[strcspn(items[i].name, "\n")] = '\0';

            printf("가격을 입력해 주세요: ");
            items[i].price = 0;
            scanf("%d", &items[i].price);
            getchar();

            printf("등록되었습니다!\n이름: %s\n가격: %d원\n", items[i].name, items[i].price);


            i++;
            
        }else if(menu == 2){

            printf("등록된 물품의 목록을 볼 수 있습니다, 계속 진행하려면 1을 뒤로가려면 5를 입력하세요.\n");

            int num = 0;
            scanf("%d", &num);
            getchar();

            if(num == 5) continue;

            for (int j = 0; j < i; j++){
                printf("id: %d\n이름: %s\n가격: %d원\n\n", items[j].id, items[j].name, items[j].price);
            }

        }else if(menu == 3){
            printf("등록된 물품을 제거할 수 있습니다, 계속 진행하려면 1을 뒤로가려면 5를 입력하세요.\n");

            int num = 0;
            scanf("%d", &num);
            getchar();

            if(num == 5) continue;

            printf("지우고 싶은 물품의 id를 적어주세요: ");
            int rmid = 0;
            scanf("%d", &rmid);

            for (int k = 0; k < i; k++){
                if( rmid == items[k].id ){
                    
                    items[k].id = 0;
                    items[k].name[0] = '\0';
                    items[k].price = 0;

                    break;
                }
            }

            printf("정상적으로 삭제 되었습니다!\n");

            i--;

        }else if(menu == 4) break;


    }
    
    return 0;
}
```
1시간 정도 걸려서 코드의 뼈대를 완성했다.

## 개선하고 싶은 점
-코드를 다 짜고 천천히 둘러보니 고쳐야 할 점들이 많이 보였다. 아래의 항목들은 차차 고쳐 나갈 문제점 들이다.
1. 프로그램 종료 시 데이터가 유지되지 않아 저장 구조(메모리/파일)가 필요함
2. 기능 추가를 고려한 CLI 메뉴 흐름과 메인 루프 설계가 정리되지 않음
3. 상품을 식별할 고유 ID 관리 전략이 없어 중복 및 삭제 이후 문제가 발생할 수 있음
4. 상품 삭제 시 배열 처리 및 파일 반영 방식에 대한 명확한 설계가 필요함
5. scanf 기반 입력 처리로 인해 공백 입력 및 잘못된 입력에 취약함
6. 초기 구조체 설계에서 추후 기능 확장을 어떻게 수용할지 기준이 없음
7. 삭제 로직이 배열 구조상 완전한 삭제가 아님
8. 고정 크기 배열과 아이템 개수 관리 방식이 불안정함
9. 메뉴 및 입력 값에 대한 유효성 검사 로직이 부족함
10. 모든 기능이 main 함수에 집중되어 있어 함수 분리가 필요함

## 느낀 점
생각보다 고려할 게 많아 구조를 잡는 데 시간이 걸렸다.
기능을 하나 추가할수록 코드가 복잡해질 것 같아
초반 설계의 중요성을 체감하였다.
