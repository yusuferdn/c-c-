# c-c-
C/C++ İŞLEMLER

#include <stdio.h>
#include <stdlib.h>

int** matrix[5]; 

int printMenu();


int** allocateMatrix(int, int);

void freeAll();

void printMatrix(int**);

void getMatrixElement(int**);

void getMatrixSize(int);

void calSizesOfResultMatrix(int);

void add();

void sub();

void transpose();

void mulWithConstant();

void multiple();

int determinant(int** , int );

int main(){
	int operation;
	char devam='e';
	
	while( devam == 'e' || devam == 'E' ){
		freeAll();
		printf("\n");
		operation = printMenu();
		
		switch( operation ){
			case 1: 
				getMatrixSize(2); 
				if( matrix[0][0][0] != matrix[1][0][0]  || matrix[0][0][1] != matrix[1][0][1] ){
					printf("HATA : toplama ve cikarma icin iki matrisin satir ve sutun sayiları esit olmalı!\n");
					continue;
				}
				add();
				printf("Verilen iki matrisin toplamı : \n");
			break;
			case 2: 
				printf("\n BILGILENDIRME : Birinci matristen ikinci matris cikartilacaktir.\n");
				getMatrixSize(2);  
				if( matrix[0][0][0] != matrix[1][0][0]  || matrix[0][0][1] != matrix[1][0][1] ){
					printf("HATA : toplama ve cikarma icin iki matrisin satir ve sutun sayiları esit olmalı!\n");
					continue;
				}
				sub();
				printf("1.Matristen 2.matrisin cikarilmasiyla olusan matris :\n");
			break;
			case 3: 
				getMatrixSize(1);
		
				mulWithConstant();
				printf("\nMatrisin sabit ile carpilmis hali : \n");
			break;
			case 4:
				getMatrixSize(1);				
				if( matrix[0][0][0] != matrix[0][0][1] ){
					printf("HATA : determinantının alinması icin kare matris olmalı! \n");
					continue;
				}
				printf("Determinant : |A| : %d \n", determinant(matrix[0],0));
			
			break;
			case 5:
				getMatrixSize(2);
				if( matrix[0][0][1] != matrix[1][0][0] ){
				printf("HATA : Carpma islemi icin 1.matrisin sutun sayisi \n        2.matrisin satir sayisina esit olmalı! \n");
				continue;
				}
				multiple();
				printf("Iki matrisin carpiminin sonucu : \n");
			break;
			case 6: 
				getMatrixSize(1);
			
				transpose();
				printf("Verilen matrisin transpozesi : \n");			
			break;
			case 0:
				return 0;
			break;
			
		} 
		
		
		if( operation != 4 ){
		
			printMatrix(matrix[2]);
		}		
		
		getchar();
		printf("\nNew operation e/h? > ");		
		devam = getchar();
	} 
	
	return 0;
}


int printMenu(){
		int op;
	
		printf("=== Matrix operations ===\n");
		printf("[1] - Toplama\n");
		printf("[2] - Cikarma\n");
		printf("[3] - Sabit ile Carpma\n");
		printf("[4] - Determinant\n");
		printf("[5] - İki matrisin carpimi\n");
		printf("[6] - Transpoze\n");
		printf("[0] - Exit\n");
		
		do{
			printf("\nHangi islemi yapmak istiyorsanız basindaki sayiyi giriniz > ");
			scanf("%d", &op);
		}while( op<0 || op>6 );
		
		return op;
}

int** allocateMatrix(int row, int col){
	int i;
	int **matrixPointer;

	row++;
	col++;
	
	
	
	matrixPointer = (int**)malloc( row * sizeof(int*) );
		
	if( matrixPointer == NULL){ printf("HATA : Gerekli alan ayrılamadi!."); exit(0); }
	
	for( i=0; i<row; i++ ){
		matrixPointer[i] = (int*)malloc( col * sizeof(int) );
		
		if( matrixPointer[i] == NULL){ printf("HATA : Gerekli alan ayrılamadi!."); exit(0); }
	}
	
	
	matrixPointer[0][0] = row;
	matrixPointer[0][1] = col;
	
	return matrixPointer;
}

void printMatrix(int** theMatrix){
	int i,j;
	
	for( i=1; i<theMatrix[0][0]; i++ ){
		printf("\n");
		for( j=1; j<theMatrix[0][1]; j++ ){
			printf(" %2d", theMatrix[i][j]);
		}
	}
	printf("\n\n");
}

void getMatrixElement(int** theMatrix){
	int i,j;
	
	printf("\n");
	for( i=1; i<theMatrix[0][0]; i++ ){
		for( j=1; j<theMatrix[0][1]; j++ ){
			printf("[%d][%d] element : ", i,j);
			scanf("%d", &theMatrix[i][j]);
		}
	}
}

void getMatrixSize(int numberOfMatrix){
	int i;
	int row,col;
	
	for( i=0; i<numberOfMatrix; i++ ){
		
		printf("\n%d.Matrisin Satir Sayisi > ", i+1);
		scanf("%d", &row);
		printf("\n%d.Matrisin Sutun Sayisi > ", i+1);
		scanf("%d", &col);
	
		matrix[i] = allocateMatrix(row, col);
		
		getMatrixElement(matrix[i]);
		
		printf("\n%d.Matris : \n", i+1);
		printMatrix(matrix[i]);
	}
}

void calSizesOfResultMatrix(int op){
	int row, col;
	switch( op ){
		case 1: 
		case 2: 
		case 3: 
			row = matrix[0][0][0] - 1; 
			col = matrix[0][0][1] - 1; 
		break;
		case 5:
			row = matrix[0][0][0] -1; 
			col = matrix[1][0][1] -1; 
		break;
		case 6:
			row = matrix[0][0][1] - 1;
			col = matrix[0][0][0] - 1;
		break;
	}
	
	matrix[2] = allocateMatrix(row, col);
}

void add(){
	int i,j;
	
	
	
	
	calSizesOfResultMatrix(1); 
	
	for( i=1; i<matrix[0][0][0]; i++ ){
		for( j=1; j<matrix[0][0][1]; j++ ){
			matrix[2][i][j] = matrix[0][i][j] + matrix[1][i][j];
		}
	}
}

void sub(){
	int i,j;
	

	calSizesOfResultMatrix(2); 
	
	for( i=1; i<matrix[0][0][0]; i++ ){
		for( j=1; j<matrix[0][0][1]; j++ ){
			matrix[2][i][j] = matrix[0][i][j] - matrix[1][i][j];
		}
	}	
}

void transpose(){
	
	int i,j;
	
	calSizesOfResultMatrix(6);
	
	for( i=1; i<matrix[0][0][0]; i++ ){
		for( j=1; j<matrix[0][0][1]; j++ ){
			matrix[2][j][i] = matrix[0][i][j];
			printf("matrix[2][%d][%d] : %d\n", j,i,matrix[2][j][i]);
		}
	}
}

void mulWithConstant(){
	int i,j,con;
	
	calSizesOfResultMatrix(3);
	
	printf("\nEnter constant > ");
	scanf("%d", &con);
	
	for( i=1; i<matrix[0][0][0]; i++ ){
		for( j=1; j<matrix[0][0][1]; j++ ){
			matrix[2][i][j] = matrix[0][i][j] * con;
		}
	}
}

void multiple(){
	int i,j,k;
	
	calSizesOfResultMatrix(5);
	
	for( i=1; i<matrix[0][0][0]; i++ ){
		for( j=1; j<matrix[1][0][1]; j++ ){
			for( k=1; k<matrix[0][0][1]; k++ ){
				matrix[2][i][j] = matrix[2][i][j] + ( matrix[0][i][k] * matrix[1][k][j] );
			}
		}
	}
}

int determinant(int** theMatrix, int derinlik){
	int det=0,isaret= -1;
	int newSat,newSut=1;
	int sut,sat,i,j;
	

	if( theMatrix[0][0] == 3 ){

		det = theMatrix[1][1] * theMatrix[2][2] - theMatrix[2][1] * theMatrix[1][2];
		return det;
	}else{ 		
		
	
		sat = 1;
		
		
		derinlik++; 
	
		matrix[derinlik] = allocateMatrix(theMatrix[0][0]-2,theMatrix[0][1]-2); 
		
		
		for( sut=1; sut<theMatrix[0][1]; sut++ ){ 
			newSat=0; 			
		
			for( i=2; i<theMatrix[0][0]; i++ ){ 		
				newSat++; 
				newSut = 1; 
				for( j=1; j<theMatrix[0][1]; j++ ){
					if( sut == j ){
						continue; 
					}
					matrix[derinlik][newSat][newSut] = theMatrix[i][j]; 
					newSut++;
				}
			}
		
			
			det = det + theMatrix[sat][sut] * isaret * determinant(matrix[derinlik], derinlik);			
			isaret = isaret * (-1); 
					
		} 
		return det;	
	} 
}

void freeAll(){
	int i;
	
	for(i=0; i<5; i++){
		free(matrix[i]);
	}
}



/////////////////////////////////////////////////////////////////////////////////////




#include <iostream>

int main() {
  int n,m=1;
  std::cout << "n degerini giriniz: ";
  std::cin >> n;
 

  float toplam=0;
  for (float i = 1; i <=m; i++) {

    for (float j = 1; j <=n; j++)
    {
      toplam=toplam+ (1/(j*j+3));
      std::cout << "Sonuc: " << toplam << std::endl;
    }
  }
   
 

  return 0;
}
  
  
 
  
  ////////////////////////////////////////////////////////////////////////
  
  #include <iostream>


using namespace std;

int main()
{
int n; // Öğrenci sayısı
cout << "Öğrenci sayısını giriniz: ";
cin >> n;


for (int i = 1; i <= n; i++) // Öğrenci sayısı kadar döngü
{
    int vize, kisa1, kisa2, final; // Öğrencinin notları
    cout << "Öğrencinin vize, 1. kısa sınav, 2. kısa sınav ve final notlarını giriniz: ";
    cin >> vize >> kisa1 >> kisa2 >> final;

    // Ağırlıkları hesapla
    double agirlikli_ortalama = (vize * 0.25) + (kisa1 * 0.1) + (kisa2 * 0.15) + (final * 0.5);

    // Durumu yazdır
    if (agirlikli_ortalama < 50)
        cout << "KALDI" << endl;
    else
        cout << "GEÇTİ" << endl;
}

return 0;
}
                               
//////////////////////////////////////////////////////////////////////////////
                               
#include <iostream>
#include <stdio.h>
#include <conio.h>
using namespace std;
int main()
{
        string ad;
        int doviz;
    int lira;  
        int alim,satim; //Alımda büro alıyor satıcı satıyor,satımda tam tersi
        int secim;
        int secim2;
    double dolar;
    double euro;
    double sterlin;
    double frank;
   
     
   
    cout << "Isminizi Giriniz: \n ";
    cin >> ad;
         
            cout << "*Anlik Doviz Kuru*" << endl;
        cout << " 1-Euro\n 2-Dolar\n 3-Sterlin\n 4-Frank\n "<< endl;
         
        cout << "*Islem Yapmak Istediginiz Doviz Kuru*" << endl;
        cout << " 1-Euro\n 2-Dolar\n 3-Sterlin\n 4-Frank\n "<< endl;
        cout << "Seciminiz :" << endl;
        cin >> secim;
        switch (secim)
        {
 
 
        case 1:
                cout << "Seciminiz Euro" << endl;
                     cout << " 1-Alim\n 2-Satim" << endl;
                     cin>> secim2;
                     switch(secim2){
                       
                                case 1:
                        cout << "Seciminiz Alim" << endl;
                       
                                cout << "Miktar :" << endl;
                            cin >>doviz;
                            cout<<"Elinizdeki Turk Lirasini Giriniz:";
                        cin>>lira;
                       
                              if(doviz<1000 && doviz>0){ euro = lira / 3.85;}
                              else if (1000<doviz && doviz>5000){ euro = lira / 3.89;}
                              else if (doviz>5000){ euro = lira / 3.90;}
                        break ;
                       
                                case 2:
                                cout << "Seciminiz Satim" << endl;
                                   
                                cout << "Miktar :" << endl;
                    cin >>doviz;
                    cout<<"Elinizdeki Turk Lirasini Giriniz:";
                cin>>lira;
                           
                                   
                                    if(doviz<1000 && doviz>0 ){euro = lira / 3.98;}
                                    else if (1000<doviz && doviz>5000){euro = lira / 3.94;}
                                    else if (doviz>5000){euro = lira / 3.92;}
                                break ;
                               
                                default:
                        cout << "Hatali Giris Yaptiniz!" << endl;
                    break ;
                         }
                break;
        case 2:
                cout << "Seciminiz Dolar" << endl;
                    cout << " 1-Alim\n 2-Satim" << endl;
                     cin>> secim2;
                     switch(secim2){
                       
                                case 1:
                        cout << "Seciminiz Alim" << endl;
                           
                                         cout << "Miktar :" << endl;
                         cin >>doviz;
                             cout<<"Elinizdeki Turk Lirasini Giriniz:";
                                         cin>>lira;
                                         
                                     if(doviz<1000 && doviz>0) {dolar= lira / 3.54;}
                             else if (1000<doviz && doviz>5000) {dolar= lira / 3.57;}
                             else if (doviz>5000) {dolar= lira / 3.58;}
                        break ;
                       
                                case 2:
                                cout << "Seciminiz Satim" << endl;
                               
                                    cout << "Miktar :" << endl;
                        cin >>doviz;  
                                        cout<<"Elinizdeki Turk Lirasini Giriniz:";
                    cin>>lira;  
                               
                                if(doviz<1000 && doviz>0 ){dolar= lira / 3.68;}
                else if (1000<doviz && doviz>5000){dolar= lira / 3.63;}
                else if (doviz>5000){dolar= lira / 3.62;}
                               
                               
                                break ;
                               
                                default:
                        cout << "Hatali Giris Yaptiniz!" << endl;
                         }
                break;
        case 3:
                cout << "Seciminiz Sterlin" << endl;
                cout << " 1-Alim\n 2-Satim" << endl;
                     cin>> secim2;
                     switch(secim2){
                       
                                case 1:
                        cout << "Seciminiz Alim" << endl;
                               
                                                cout << "Miktar :" << endl;
                            cin >>doviz;
                                                cout<<"Elinizdeki Turk Lirasini Giriniz:";
                        cin>>lira;  
                       
                              if(doviz<1000 && doviz>0){sterlin = lira / 4.57;}
                              else if (1000<doviz && doviz>5000){sterlin = lira / 4.57;}
                              else if (doviz>5000){sterlin = lira / 4.60;}
                        break ;
                       
                                case 2:
                                cout << "Seciminiz Satim" << endl;
                               
                                     cout << "Miktar :" << endl;
                         cin >>doviz;
                         cout<<"Elinizdeki Turk Lirasini Giriniz:";
                     cin>>lira;
                               
                                if(doviz<1000 && doviz>0 ){sterlin = lira / 4.70;}
                else if (1000<doviz && doviz>5000){sterlin = lira / 4.66;}
                else if (doviz>5000){sterlin = lira / 4.62;}
                               
                               
                               
                                break ;
                               
                                default:
                        cout << "Hatali Giris Yaptiniz!" << endl;
                         }
                break;
        case 4:
                cout << "Seciminiz Frank" << endl;
                cout << " 1-Alim\n 2-Satim" << endl;
                     cin>> secim2;
                     switch(secim2){
                       
                                case 1:
                        cout << "Seciminiz Alim" << endl;
                       
                              cout << "Miktar :" << endl;
                          cin >>doviz;
                          cout<<"Elinizdeki Turk Lirasini Giriniz:";
                      cin>>lira;
                       
                             if(doviz<1000 && doviz>0) {frank = lira / 3.55;}
                             else if (1000<doviz && doviz>5000) {frank = lira / 3.61;}
                             else if (doviz>5000) {frank = lira / 3.67;}
                        break ;
                       
                                case 2:
                                cout << "Seciminiz Satim" << endl;
                               
                                     cout << "Miktar :" << endl;
                         cin >>doviz;
                         cout<<"Elinizdeki Turk Lirasini Giriniz:";
                     cin>>lira;
                               
                                if(doviz<1000 && doviz>0 ){frank = lira / 3.75;}
                else if (1000<doviz && doviz>5000){frank = lira / 3.70;}
                else if (doviz>5000){frank = lira / 3.69;}
                               
                               
                                break ;
                               
                                default:
                        cout << "Hatali Giris Yaptiniz!" << endl;
                         }
                break;
        default:
                cout << "Hatali Giris Yaptiniz!" << endl;
        }
}
  
  //////////////////////////////////////////////////////////////////////////


