# PROBLEM STATEMENT
# https://www.codechef.com/JULY16/problems/CHSGMNTS/

# SOLUTION
    #include <iostream>
 
    using namespace std;
    bool binarysearch(long int **co,int l,int h,long int x);
    int binarysearchpos(long int **co,int l,int h,long int x);
    int main()
    {
        ios_base::sync_with_stdio(false);
        int t,n,i,l,j,top,b,p,h;
        bool found;
        long long int c,count;
        cin>>t;
        while(t--)
        {
            cin>>n;
            long int a[n],cj[n],**co,cou[n];
            co=new long int*[n];
            count=0;
            top=0;
            for(i=0;i<n;i++)
            {
                co[i]=new long int[2];
                cin>>a[i];
                if(i==0)
                {
                    co[0][0]=a[i];
                    co[0][1]=1;
                }
                else
                {
                    found=binarysearch(co,0,top,a[i]);
                    if(found)
                    {
                        j=top;
                        while(co[j][0]!=a[i])
                        {
                            j--;
                        }
                        co[j][1]++;
                    }
                    else
                    {
                        j=top;
                        while(co[j][0]>a[i])
                        {
                            co[j+1][0]=co[j][0];
                            co[j+1][1]=co[j][1];
                            j--;
                            if(j<0)
                                break;
                        }
                        co[j+1][0]=a[i];
                        co[j+1][1]=1;
                        top++;
                    }
                }
            }
            b=0;
            for(i=0;i<n-1;i++)
            {
                if(b==0)
                {
                    p=binarysearchpos(co,0,top,a[i]);
                    if(co[p][1]==1)
                        b=1;
                    co[p][1]--;
                    for(j=i;j<=n-1;j++)
                    {
                        cou[j-i]=0;
                        cj[j]=0;
                    }
                    for(l=0;i+l<n-1;l++)
                    {
                        c=0;
                        h=1;
                        if(cj[i+l]==1)
                        {
                            h=0;
                            cou[l]=cou[l-1];
                        }
                        else{
                        for(j=i+l+1;j<=n-1;j++)
                        {
                            if(a[j]==a[i+l])
                            {
                                cj[j]=1;
                                cou[l]+=((c*(c+1))/2);
                                c=0;
                            }
                            else if(cj[j]==1)
                            {
                                cou[l]+=((c*(c+1))/2);
                                c=0;
                            }
                            else if(h==1)
                            {
                                h=0;
                                c++;
                            }
                            else
                                c++;
                        }
                        if(c>0)
                            cou[l]+=((c*(c+1))/2);
                        }
                        if(h==1)
                            break;
                    }
                }
                else
                {
                    p=binarysearchpos(co,0,top,a[i]);
                    if(co[p][1]>1)
                        b=0;
                    co[p][1]--;
                    for(j=i;j<=n-1;j++)
                        cou[j-i]=cou[j-i+1];
                }
                for(j=i;j<=n-1;j++)
                    count+=cou[j-i];
            }
            cout<<count<<"\n";
        }
        return 0;
    }
    bool binarysearch(long int **co,int l,int h,long int x)
    {
        int m;
        bool f;
        m=(l+h)/2;
        if(co[m][0]==x)
            return true;
        else if(l==h)
            return false;
        else
        {
            if(co[m][0]<x)
            {
                if(m+1<=h)
                    f=binarysearch(co,m+1,h,x);
            }
            else
            {
                if(m-1>=l)
                    f=binarysearch(co,l,m-1,x);
            }
        }
        return f;
    }
    int binarysearchpos(long int **co,int l,int h,long int x)
    {
        int m;
        int f;
        m=(l+h)/2;
        if(co[m][0]==x)
            return m;
        else
        {
            if(co[m][0]<x)
            {
                if(m+1<=h)
                    f=binarysearchpos(co,m+1,h,x);
            }
            else
            {
                if(m-1>=l)
                    f=binarysearchpos(co,l,m-1,x);
            }
        }
        return f;
    }
